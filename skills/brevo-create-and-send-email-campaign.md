---
name: Create and send a Brevo email campaign
description: Build, test and send a marketing email campaign through the Brevo Marketing Campaigns API — including recipient targeting by list and segment, A/B tests, UTM customization, and the states in which a campaign can no longer be modified.
api: openapi/brevo-marketing-campaigns-openapi.yml
operations:
  - createEmailCampaign
  - getEmailCampaign
  - updateEmailCampaign
  - sendTestEmail
  - sendEmailCampaignNow
  - updateCampaignStatus
  - getAbTestCampaignResult
  - emailExportRecipients
  - deleteEmailCampaign
generated: '2026-08-13'
method: generated
source: openapi/brevo-marketing-campaigns-openapi.yml + changelog/brevo-changelog.yml + errors/brevo-problem-types.yml
---

# Create and send a Brevo email campaign

Base URL `https://api.brevo.com/v3`, `api-key` header. Under OAuth this needs
`campaigns.email:write` (and `campaigns.email:read` to read results back — `:write` does
not imply `:read`). Marketing campaigns are **not** transactional email; do not use
`sendTransacEmail` for a broadcast.

## 1. Create the campaign

`createEmailCampaign` — `POST /emailCampaigns`. The essentials:

- `name`, `subject`, `sender` (`{name, email}` — the address must be a verified sender;
  see `getSenders`)
- content: `htmlContent`, `htmlUrl`, or `templateId`
- `recipients`: `{ listIds: [...], exclusionListIds: [...], segmentIds: [...],
  exclusionSegmentIds: [...] }`
- optional `scheduledAt` to schedule instead of sending now

Optional UTM overrides (added 2026-07-20): `utmCampaign`, `utmContent`, `utmTerm`.
`utmCampaign` accepts alphanumerics and spaces only, and defaults to the campaign name.

## 2. Send yourself a test first

`sendTestEmail` — `POST /emailCampaigns/{campaignId}/sendTest` with `emailTo[]`. This is
the only rendering check available; there is no campaign preview endpoint and no sandbox
mode on the campaign surface.

If a test address is not reachable Brevo returns `405` with the failing addresses named.

## 3. Send or schedule

- Immediate: `sendEmailCampaignNow` — `POST /emailCampaigns/{campaignId}/sendNow`
- Scheduled: set `scheduledAt` on create/update, then drive state with
  `updateCampaignStatus` — `PUT /emailCampaigns/{campaignId}/status`

## 4. Know when the campaign locks

`updateEmailCampaign` and `deleteEmailCampaign` fail once sending has begun:

- `campaign_processing` — the campaign is being processed; it cannot be modified now
- `campaign_sent` — already sent; it is immutable

Both arrive as `400`. Treat them as terminal, not retryable — re-reading and re-sending
would duplicate a broadcast to the whole list.

`402 not_enough_credits` means the account cannot cover the send. Also terminal: it is a
billing state, not a transient error.

## 5. Read results

- `getEmailCampaign` / `getEmailCampaigns` — statistics are returned inline on the campaign
  object; `getEmailCampaigns` pages with `limit`/`offset`/`sort` and filters on `type`,
  `status` and date range.
- `getAbTestCampaignResult` — `GET /emailCampaigns/{campaignId}/abTestCampaignResult`, only
  for campaigns created with A/B testing enabled.
- `emailExportRecipients` — asynchronous; returns a `processId` to poll.

Do not poll for results in a loop. Campaign endpoints sit in the 100 requests/hour default
bucket; use marketing webhooks for delivery, open, click and unsubscribe events.
