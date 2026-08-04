# brevo (brevo)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Send transactional emails with static or dynamic content using the Messaging API.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/brevo/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/brevo/refs/heads/main/apis.yml)

## Timestamps

- **Modified:** 2026-05-19

## APIs

### Brevo Transactional Email API

The Brevo Transactional Email API allows developers to send transactional emails such as order confirmations, password resets, and account notifications programmatically. It supports single and batch sending, scheduled deliveries, template-based emails, and attachment handling. The API also provides endpoints for tracking email activity including opens, clicks, bounces, and delivery status through detailed event logs and real-time webhooks.

- **Human URL:** [https://developers.brevo.com/docs/send-a-transactional-email](https://developers.brevo.com/docs/send-a-transactional-email)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Email
- Messaging
- SMTP
- Transactional

#### Properties

- [Documentation](https://developers.brevo.com/docs/send-a-transactional-email)
- [OpenAPI](openapi/brevo-transactional-email-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-transactional-email.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-transactional-email.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo Email Campaigns API

The Brevo Email Campaigns API enables developers to create, manage, and send marketing email campaigns programmatically. It provides endpoints for building campaigns with HTML content or templates, scheduling sends, segmenting audiences, and managing sender identities. Developers can retrieve campaign statistics including open rates, click rates, and unsubscribes to measure performance and optimize future campaigns.

- **Human URL:** [https://developers.brevo.com/docs/getting-started](https://developers.brevo.com/docs/getting-started)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Automation
- Campaigns
- Email
- Marketing

#### Properties

- [Documentation](https://developers.brevo.com/docs/getting-started)
- [OpenAPI](openapi/brevo-email-campaigns-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-email-campaigns.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-email-campaigns.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo Contacts API

The Brevo Contacts API provides programmatic access to contact management features including creating, updating, and deleting contacts. Developers can organize contacts into lists, apply attributes and tags, import contacts in bulk, and build audience segments for targeted campaigns. The API also supports managing folders, contact attributes, and custom fields to structure contact data according to business needs.

- **Human URL:** [https://developers.brevo.com/docs/how-it-works](https://developers.brevo.com/docs/how-it-works)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Contacts
- CRM
- Lists
- Segmentation

#### Properties

- [Documentation](https://developers.brevo.com/docs/how-it-works)
- [OpenAPI](openapi/brevo-contacts-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-contacts.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-contacts.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo Transactional SMS API

The Brevo Transactional SMS API allows developers to send non-promotional SMS messages such as order confirmations, delivery notifications, and verification codes using recipients' phone numbers. It supports sending individual messages with customizable sender names and provides endpoints for tracking SMS delivery status and activity. The API is designed for time-sensitive notifications that require immediate delivery to mobile devices.

- **Human URL:** [https://developers.brevo.com/docs/transactional-sms-endpoints](https://developers.brevo.com/docs/transactional-sms-endpoints)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Messaging
- Mobile
- SMS
- Transactional

#### Properties

- [Documentation](https://developers.brevo.com/docs/transactional-sms-endpoints)
- [OpenAPI](openapi/brevo-transactional-sms-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-transactional-sms.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-transactional-sms.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo WhatsApp API

The Brevo WhatsApp API enables developers to send transactional WhatsApp messages such as order confirmations, status updates, and password reset links through the WhatsApp Business platform. It provides endpoints for sending template-based messages, managing WhatsApp campaigns, and tracking message delivery and read status. The API leverages WhatsApp's high engagement rates to deliver important notifications directly to users on their preferred messaging platform.

- **Human URL:** [https://developers.brevo.com/docs/whatsapp-messages](https://developers.brevo.com/docs/whatsapp-messages)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Messaging
- Mobile
- Transactional
- WhatsApp

#### Properties

- [Documentation](https://developers.brevo.com/docs/whatsapp-messages)
- [OpenAPI](openapi/brevo-whatsapp-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-whatsapp.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-whatsapp.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo eCommerce API

The Brevo eCommerce API allows developers to sync product catalogs, categories, and order data with the Brevo platform. It provides endpoints for importing and managing products, organizing them into categories, and tracking customer purchase history. This data integration enables merchants to attribute revenue to marketing campaigns, trigger automated workflows based on purchase behavior, and build product recommendation segments for targeted messaging.

- **Human URL:** [https://developers.brevo.com/docs/import-your-products](https://developers.brevo.com/docs/import-your-products)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Categories
- Ecommerce
- Orders
- Products

#### Properties

- [Documentation](https://developers.brevo.com/docs/import-your-products)
- [OpenAPI](openapi/brevo-ecommerce-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-ecommerce.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-ecommerce.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo Conversations API

The Brevo Conversations API provides programmatic access to live chat and messaging features for customer support and engagement. It enables developers to manage chat conversations, send and receive messages, and integrate Brevo's chat widget into websites and applications. The API supports real-time communication with site visitors and can be used to build custom chat interfaces, automate responses, and route conversations to appropriate team members.

- **Human URL:** [https://developers.brevo.com/docs/getting-started](https://developers.brevo.com/docs/getting-started)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Chat
- Conversations
- Live Chat
- Support

#### Properties

- [Documentation](https://developers.brevo.com/docs/getting-started)
- [OpenAPI](openapi/brevo-conversations-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-conversations.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-conversations.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Brevo Webhooks API

The Brevo Webhooks API allows developers to receive real-time notifications when events occur across transactional emails, marketing campaigns, and conversations. By configuring webhook subscriptions, applications can automatically receive data for events such as email deliveries, opens, clicks, bounces, spam reports, and unsubscribes. This eliminates the need for polling and enables event-driven integrations that respond immediately to changes in messaging activity.

- **Human URL:** [https://developers.brevo.com/docs/transactional-webhooks](https://developers.brevo.com/docs/transactional-webhooks)
- **Base URL:** `https://api.brevo.com/v3`

#### Tags

- Automation
- Events
- Notifications
- Webhooks

#### Properties

- [Documentation](https://developers.brevo.com/docs/transactional-webhooks)
- [OpenAPI](openapi/brevo-webhooks-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/brevo-webhooks.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/brevo-webhooks.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [AsyncAPI](asyncapi/brevo-webhooks-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)

## Common Properties

- [GitHub Organization](https://github.com/getbrevo)
- [LinkedIn](https://www.linkedin.com/company/brevo)
- [JSON-LD](json-ld/brevo-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON Schema](json-schema/brevo-contact-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/brevo-email-event-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/brevo-order-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [L L Ms Txt](https://developers.brevo.com/llms.txt)
