---
name: Run a Brevo loyalty program
description: Stand up a Brevo loyalty program end to end — create and publish the program, define a points balance, enroll a contact, credit and debit points through the two-phase transaction lifecycle, and issue and redeem a voucher.
api: openapi/brevo-loyalty-openapi.yml
operations:
  - createNewLP
  - publishLoyaltyProgram
  - createBalanceDefinition
  - subscribeToLoyaltyProgram
  - beginTransaction
  - completeTransaction
  - cancelTransaction
  - getContactBalances
  - createReward
  - createVoucher
  - redeemVoucher
  - completeRedeemTransaction
  - createTierGroup
  - addSubscriptionToTier
generated: '2026-08-13'
method: generated
source: openapi/brevo-loyalty-openapi.yml + conventions/brevo-conventions.yml
---

# Run a Brevo loyalty program

Base URL `https://api.brevo.com/v3`, `api-key` header; under OAuth, `loyalty:read` and
`loyalty:write`. Loyalty is Brevo's newest surface — 50 operations across four families
(program, balance, reward, tier) — and it has **no MCP module**, so an agent must drive it
over REST.

Rate limit note: everything under `/v3/loyalty/{...}` shares 600 requests/hour on standard
accounts (1,200 Advanced, 100,000 Extended). Budget accordingly; this is far tighter than
the transactional surface.

## 1. Create and publish the program

1. `createNewLP` — `POST /loyalty/config/programs`. Returns the program id (`pid`), a UUID
   that every subsequent path needs.
2. `publishLoyaltyProgram` — `POST /loyalty/config/programs/{pid}/publish`. Until published
   the program is not live.

A `409` on create means the program name is already taken.

## 2. Define what you are counting

`createBalanceDefinition` — `POST /loyalty/balance/programs/{pid}/balance-definitions`.
This is the unit (points, stars, credits). Optionally cap it with `createBalanceLimit` on
`.../balance-definitions/{bdid}/limits`.

## 3. Enroll the contact

`subscribeToLoyaltyProgram` — `POST /loyalty/config/programs/{pid}/subscriptions`, keyed on
a Brevo `contactId`. The contact must already exist (see the contacts skill). The result is
a `loyaltySubscriptionId`, which is the handle for balances and tiers.

## 4. Credit or debit points — two phases, always

Brevo models points movement as a **two-phase transaction**. Do not treat `beginTransaction`
as complete:

1. `beginTransaction` — `POST /loyalty/balance/programs/{pid}/transactions`. Returns `tid`.
2. Then exactly one of:
   - `completeTransaction` — `POST .../transactions/{tid}/complete`
   - `cancelTransaction` — `POST .../transactions/{tid}/cancel`

A transaction left in the begun state is neither applied nor released. If your process can
fail between the two calls, persist the `tid` before calling begin, and reconcile on
restart with `getTransactionHistoryApi`.

Read balances with `getContactBalances`
(`GET /loyalty/balance/programs/{pid}/contact-balances`) or `getActiveBalancesApi`.

## 5. Rewards and vouchers

Same two-phase discipline:

1. `createReward` — `POST /loyalty/offer/programs/{pid}/offers`
2. `createVoucher` — `POST /loyalty/offer/programs/{pid}/rewards/attribute`, attributing a
   voucher to a contact
3. `redeemVoucher` — `POST /loyalty/offer/programs/{pid}/rewards/redeem`, returns `tid`
4. `completeRedeemTransaction` — `POST .../rewards/redeem/{tid}/complete`

`revokeVouchers` (`DELETE .../rewards/revoke`) withdraws unredeemed vouchers.
`validateReward` checks eligibility before you attribute anything.

## 6. Tiers

`createTierGroup` — `POST /loyalty/tier/programs/{pid}/tier-groups`, then
`createTierForTierGroup` on `.../tier-groups/{gid}/tiers`. Assign with
`addSubscriptionToTier` — `POST /loyalty/tier/programs/{pid}/contacts/{cid}/tiers/{tid}`.

## 7. Deliver the pass

`getWalletPassInstallUrl` (Wallet API,
`GET /wallet/passes/{passId}/installUrl/{contactId}`) returns a per-contact Apple/Google
Wallet install URL with the identifiers encoded in an encrypted token, so it is safe to
send by email, SMS or QR code.

## Errors

`422` (Validation errors) is the dominant failure on this surface — the loyalty specs
declare far more 422s than 400s. `409` means a uniqueness conflict (duplicate program name,
owner contact added as a member). `424 Failed Dependency` means a prerequisite object —
a balance definition, a published program — does not exist yet.
