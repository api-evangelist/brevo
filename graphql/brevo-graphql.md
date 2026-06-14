# Brevo (Sendinblue) GraphQL Schema

## Overview

This document describes the conceptual GraphQL schema for the Brevo (formerly Sendinblue) API v3. Brevo is an all-in-one marketing platform providing email campaigns, transactional email, SMS messaging, WhatsApp messaging, contact management, live chat, and eCommerce integrations.

**API Base URL:** `https://api.brevo.com/v3`
**Developer Documentation:** https://developers.brevo.com/reference

## Schema Source

This GraphQL schema is derived from the Brevo REST API v3 specification. Brevo does not natively expose a GraphQL endpoint; this schema represents a conceptual translation of the REST API surface into GraphQL types, queries, and mutations for use in tooling, code generation, and developer experience enrichment.

## Core Domain Areas

### Contacts

The contacts domain covers management of individual contacts, their attributes, email addresses, phone numbers, list membership, and bulk import operations.

- `Contact` — A contact record with email, attributes, and list associations
- `ContactDetails` — Expanded contact information including opt-in status and engagement history
- `ContactEmail` — Email address record for a contact
- `ContactPhone` — Phone number record associated with a contact
- `ContactAttributes` — Key-value attribute map for custom contact fields
- `ContactList` — The lists a contact belongs to
- `ContactImport` — Bulk import job for contacts via file upload or JSON

### Lists and Folders

- `List` — A contact list used for campaign targeting and segmentation
- `ListDetails` — Extended list info including subscriber counts
- `ListContact` — A contact entry within a list with subscription status
- `Folder` — Organizational container grouping multiple lists
- `FolderDetails` — Folder metadata including list count and creation time

### Campaigns

The campaigns domain covers email marketing campaigns from creation through scheduling, sending, and reporting.

- `Campaign` — An email marketing campaign
- `CampaignDetails` — Full campaign configuration including content, sender, and recipients
- `CampaignStatus` — Enum of lifecycle states: draft, queued, sent, suspended, archive, inProcess
- `CampaignType` — Enum distinguishing classic vs. trigger campaigns
- `CampaignSchedule` — Scheduling configuration for a campaign send
- `CampaignContent` — HTML/text body and subject line for a campaign

### Templates

- `Template` — A reusable email template
- `TemplateDetails` — Full template record including content and variable definitions
- `TemplateParams` — Template parameter definitions for dynamic content

### Statistics and Reporting

- `Statistics` — Aggregate statistics root type
- `CampaignStatistics` — Per-campaign engagement metrics
- `EmailStatistics` — Email delivery and engagement stats (opens, clicks, bounces)
- `SMSStatistics` — SMS delivery stats (sent, delivered, failed)
- `WhatsAppStatistics` — WhatsApp message delivery and read stats

### Transactional Messaging

- `Transactional` — Root type for transactional messaging resources
- `TransactionalEmail` — A single transactional email send record
- `EmailDetails` — Full details of an outbound transactional email
- `SMSMessage` — A transactional SMS message record
- `SMSDetails` — Delivery and content details for an SMS
- `WhatsAppMessage` — A transactional WhatsApp message record
- `WhatsAppDetails` — Template and delivery details for a WhatsApp message

### SMS Campaigns

- `SmsCampaign` — An SMS marketing campaign
- `SmsCampaignDetails` — Full SMS campaign record including content and recipient list

### WhatsApp

- `WhatsApp` — Root type for WhatsApp campaign resources
- `WhatsAppCampaign` — A WhatsApp campaign record
- `WhatsAppCampaignDetails` — Full WhatsApp campaign with template and audience details

### Conversations

- `Conversation` — A live chat or support conversation thread
- `ConversationDetails` — Full conversation record including agent assignment and status
- `ConversationMessage` — An individual message within a conversation

### Senders

- `Sender` — An email sender identity (name and email address)
- `SenderDetails` — Full sender record including validation status and associated IPs

### Account

- `Account` — The authenticated Brevo account record
- `AccountDetails` — Account metadata including company info, timezone, and limits
- `AccountPlan` — Subscription plan details including email and SMS quotas

### Webhooks

- `Webhook` — A webhook subscription for event notifications
- `WebhookDetails` — Full webhook record including URL and subscribed events
- `WebhookEvent` — Enum of subscribable event types (delivered, opened, clicked, bounced, etc.)

### Files and Uploads

- `File` — A file stored in Brevo (used for campaign attachments or contact imports)
- `FileDetails` — File metadata including size, MIME type, and upload timestamp
- `Upload` — An in-progress or completed file upload record

### Tracking and Events

- `Track` — Event tracking configuration
- `TrackEvent` — A tracked behavioral event associated with a contact

### Authentication

- `APIKey` — An API key record for programmatic access
- `Token` — An authentication token with expiry

### Error Handling

- `Error` — Standard error response with code and message

## Queries

- `contacts` — List contacts with pagination and filtering
- `contact(id: ID!)` — Get a single contact by ID or email
- `lists` — List all contact lists
- `list(id: ID!)` — Get a single list
- `folders` — List all folders
- `folder(id: ID!)` — Get a single folder
- `campaigns` — List email campaigns with status filter
- `campaign(id: ID!)` — Get a single campaign
- `templates` — List email templates
- `template(id: ID!)` — Get a single template
- `senders` — List sender identities
- `transactionalEmails` — List transactional email activity
- `smsCampaigns` — List SMS campaigns
- `webhooks` — List webhook subscriptions
- `account` — Get account details and plan info
- `statistics` — Get aggregate platform statistics

## Mutations

- `createContact` — Create a new contact
- `updateContact` — Update contact attributes or list membership
- `deleteContact` — Remove a contact
- `importContacts` — Bulk import contacts from file or JSON
- `createList` — Create a new contact list
- `updateList` — Rename or modify a list
- `deleteList` — Remove a list
- `addContactToList` — Add contacts to a list
- `removeContactFromList` — Remove contacts from a list
- `createCampaign` — Create a new email campaign
- `updateCampaign` — Update campaign content or settings
- `deleteCampaign` — Delete a campaign
- `scheduleCampaign` — Schedule a campaign for future send
- `sendCampaignNow` — Send a campaign immediately
- `createTemplate` — Create a new email template
- `updateTemplate` — Update template content
- `deleteTemplate` — Remove a template
- `sendTransactionalEmail` — Send a transactional email
- `sendSMS` — Send a transactional SMS
- `sendWhatsAppMessage` — Send a transactional WhatsApp message
- `createSmsCampaign` — Create an SMS campaign
- `createWebhook` — Register a new webhook
- `updateWebhook` — Update webhook URL or events
- `deleteWebhook` — Remove a webhook subscription
- `createSender` — Register a new sender identity
- `deleteSender` — Remove a sender identity
- `createFolder` — Create a new folder
- `deleteFolder` — Remove a folder
- `createAPIKey` — Generate a new API key

## Type Counts

This schema defines 65 named types covering the full Brevo API v3 surface area.
