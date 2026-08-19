---
name: Sync contacts and lists into Brevo
description: Create, update and bulk-import contacts into Brevo lists without tripping the hourly rate limit or the retired batch endpoint — including custom attributes, list membership and the consent-group surface.
api: openapi/brevo-contact-management-openapi.yml
operations:
  - getContacts
  - createContact
  - getContactInfo
  - updateContact
  - importContacts
  - requestContactExport
  - getLists
  - createList
  - addContactToList
  - removeContactFromList
  - getAttributes
  - createAttribute
generated: '2026-08-13'
method: generated
source: openapi/brevo-contact-management-openapi.yml + conventions/brevo-conventions.yml + lifecycle/brevo-lifecycle.yml
---

# Sync contacts and lists into Brevo

Base URL `https://api.brevo.com/v3`, `api-key` header. Under OAuth this flow needs both
`contacts:read` and `contacts:write` — `:write` does **not** imply `:read`.

## 1. Define your attributes first

Custom fields must exist before a contact can carry them. `getAttributes`
(`GET /contacts/attributes`) lists what the account has;
`createAttribute` (`PUT`-style `POST /contacts/attributes/{attributeCategory}/{attributeName}`)
adds one.

For category-type attributes read `valueStr`, not `value` — since 2026-05-01 the integer
`value` returns `0` for non-numeric entries like `"en"`, so it can no longer be used as a
unique key.

## 2. Create the list

`createList` (`POST /contacts/lists`) needs a `folderId`; get one from `getFolders`. Keep
the returned `id` — every membership call is keyed on it.

## 3. Bulk import — do not loop

**Never iterate `createContact` over a file.** All `/contacts/{...}` endpoints share a
10 RPS / 36,000 RPH budget, and a per-record loop burns it.

Use `importContacts` (`POST /contacts/import`) with either a `fileUrl`, a `fileBody` (CSV),
or a `jsonBody` array. It also accepts `listIds` to place everyone at once, and
`consentGroupIds` where the Consent Groups feature is enabled.

> `POST /contacts/batch` (Update Multiple Contacts) was deprecated on 2026-05-12, effective
> **2026-10-30**, and is already absent from the published spec. Migrate any use of it to
> `importContacts`.

Import is asynchronous — it returns a `processId`. Poll `getProcess`
(`GET /processes/{processId}`, in the Accounts and Settings API) rather than re-importing.

## 4. Single-record work

- `createContact` — `POST /contacts`. Pass `forceMerge: true` with `getId: true` to get
  back the surviving contact's `id` after a merge.
- `getContactInfo` / `updateContact` / `deleteContact` — `/contacts/{identifier}`. The
  identifier is polymorphic: numeric id, `email_id`, `phone_id`, `whatsapp_id`,
  `landline_number_id` or `ext_id`.
- `addContactToList` / `removeContactFromList` —
  `POST /contacts/lists/{listId}/contacts/add` and `.../remove`, both taking arrays.

## 5. Read back in pages

`getContacts` takes `limit` (default 50), `offset` (default 0) and `sort` (`asc`/`desc`),
plus `modifiedSince` / `createdSince` for incremental syncs — always prefer an incremental
filter over a full page walk, again because of the hourly budget. The response carries
`count` for the total.

For a full extract use `requestContactExport` (`POST /contacts/export`), which is
asynchronous and returns a `processId`.

## 6. Errors

`document_not_found` (404) means the list or contact id is wrong. `duplicate_parameter`
(400) on create means the email already exists — switch to `updateContact` or pass
`updateEnabled: true` on import. `403` with `CONSENT_GROUP_NOT_ENABLED` means the account
does not have consent groups turned on; drop `consentGroupIds` and retry.
