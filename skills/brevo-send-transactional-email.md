---
name: Send a transactional email with Brevo
description: Send a single or batched transactional email through the Brevo Email API, safely — with sandbox mode for testing, an idempotency key so a retry cannot double-send, and the right handling for suppressed recipients and rate limits.
api: openapi/brevo-email-api-openapi.yml
operations:
  - sendTransacEmail
  - getScheduledEmailById
  - deleteScheduledEmailById
  - getTransacEmailsList
  - getTransacBlockedContacts
  - unblockOrResubscribeATransactionalContact
generated: '2026-08-13'
method: generated
source: openapi/brevo-email-api-openapi.yml + conventions/brevo-conventions.yml + sandbox/brevo-sandbox.yml
---

# Send a transactional email with Brevo

Base URL: `https://api.brevo.com/v3`. Authenticate with the `api-key` request header —
lowercase, no `Bearer` prefix. There is no test key: the same key sends real mail.

## 1. Test the call before you send anything

Brevo has no sandbox environment. The only dry-run mechanism is a header set **inside the
JSON body's `headers` object**, not as an HTTP header:

```json
"headers": { "X-Sib-Sandbox": "drop" }
```

With `drop`, no email is delivered, no log row is written, and you still get a `201` with a
`messageId`. Use it on every first run. It validates request shape only — it does not
render templates or fire webhooks.

## 2. Make the send idempotent

Any retry without a key can send the message twice. Add a UUID `idempotencyKey`, again
inside the body's `headers` object:

```json
"headers": { "idempotencyKey": "b52dbf00-81dd-4a08-b807-085c1f0e9a11" }
```

- TTL is **30 minutes**. Reuse the same key when retrying the same request.
- A replay inside the TTL returns a `duplicate_parameter` error and is **not** processed —
  Brevo refuses the call rather than replaying the original response. Treat
  `duplicate_parameter` as "already sent", not as a failure.
- One key covers an entire `messageVersions` batch, however many versions it holds.

## 3. Send

`sendTransacEmail` — `POST /smtp/email`. Either inline content or a `templateId`:

```bash
curl -X POST https://api.brevo.com/v3/smtp/email \
  -H 'api-key: YOUR_API_KEY' \
  -H 'content-type: application/json' \
  -d '{
    "sender": {"name":"Acme","email":"noreply@acme.com"},
    "to": [{"email":"user@example.com","name":"User"}],
    "subject": "Confirm your email",
    "templateId": 1,
    "params": {"greeting":"Welcome"},
    "headers": {"idempotencyKey":"<uuid>"}
  }'
```

For batches use `messageVersions[]` — up to 1000 personalized versions in one request. The
response returns one message ID per version.

## 4. Handle the failure modes that actually happen

| Status | `code` | What to do |
|---|---|---|
| 400 | `invalid_parameter` / `missing_parameter` | Fix the payload; do not retry as-is |
| 400 | `duplicate_parameter` | Idempotency replay — the original send already happened |
| 401 | `unauthorized` | `api-key` header missing or wrong |
| 402 | `not_enough_credits` | Billing condition, not transient. Do **not** retry |
| 429 | — | Rate limited. See below |

## 5. Respect the rate limit signal

Every response carries `x-sib-ratelimit-limit`, `x-sib-ratelimit-remaining` and
`x-sib-ratelimit-reset` (seconds). There is **no `Retry-After`** — back off for
`x-sib-ratelimit-reset` seconds. `POST /smtp/email` allows 1,000 RPS on standard accounts;
almost every other endpoint is capped at **100 requests per hour**, so do not loop reads
around a send.

## 6. Scheduled and suppressed recipients

- Schedule by setting `scheduledAt`; inspect with `getScheduledEmailById`
  (`GET /smtp/emailStatus/{identifier}`) and cancel with `deleteScheduledEmailById`.
- A recipient may be suppressed. `getTransacBlockedContacts` returns the reason —
  `unsubscribedViaMA`, `unsubscribedViaEmail`, `unsubscribedViaApi`, `adminBlocked`,
  `hardBounce`, `contactFlaggedAsSpam`. Only `unblockOrResubscribeATransactionalContact`
  clears it, and only where consent genuinely permits.
- Do not poll `getTransacEmailsList` for delivery state — it sits in the 300 requests/hour
  bucket. Subscribe to transactional webhooks instead.
