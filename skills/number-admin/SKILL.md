---
name: number-admin
description: Manage an app2email WhatsApp Business number - message templates (create, edit, fix rejected ones), business profile and display name, contacts, blocked users and the Do Not Call list, webhooks, QR codes, number health, usage and analytics. Use when the user wants to change or check anything about their WhatsApp number or its templates rather than send a message.
---

# Manage a WhatsApp number on app2email

Use the tools of the **app2email** connector. They work on the one WhatsApp number the user connected. Changing settings needs a connection with "everything" access: if a tool answers that the permission is missing, tell the user to reconnect the app2email connector and choose "everything".

Tools that change something are marked destructive. Before calling one, say in one line what will change and get the user's go-ahead, unless they already asked for exactly that change. Afterwards, read the setting back and report it.

## Start

`check_key` shows the number, the plan, the limits and the access. `get_number_health` shows whether the number can send, its quality rating, its messaging limit and its display name status. Start there when the user says messages are not arriving.

## Templates

- `list_templates` and `get_template`: status (APPROVED, PENDING, REJECTED, PAUSED), the rejection reason, and what each template needs.
- `create_template` writes a new template and submits it to Meta for review (usually minutes, sometimes up to a day). Pick the category honestly: MARKETING for promotions, UTILITY for updates about something the customer asked for or bought, AUTHENTICATION for one-time codes (`upsert_auth_template`, `preview_auth_templates`). A UTILITY template with promotional wording is re-filed as MARKETING by Meta.
- `list_template_library` lists Meta's pre-written utility and authentication templates.
- `edit_template` changes a template and sends it for review again. `compare_templates` compares two templates by block rate and sends.
- Fixing a rejected template: read the reason with `get_template`, change only what the reason names, and edit or create it again.
- `archive_template`, `unarchive_template`, `delete_template` and `delete_templates`: deleting cannot be undone, and a deleted name cannot be reused for 30 days, so confirm first.
- `get_template_insights` (after `enable_template_insights`) shows how a template performs.

## Business profile

`get_profile` and `update_profile` (about text, description, address, e-mail, websites, category), `set_profile_photo`, `set_display_name` (Meta reviews a new display name), `get_username` and `set_username`, `get_conversation_starters` and `set_conversation_starters`.

## Contacts and consent

- `list_contacts` and `upsert_contact` for the contact list: name, tags, custom fields (used as template values when sending to a segment) and marketing consent with its evidence (who agreed, where and when). Keep consent records accurate.
- `list_donotcall`, `add_donotcall`, `remove_donotcall`: numbers on the Do Not Call list get no marketing templates. Only remove a number when the contact asked to hear from the business again.
- `list_blocked_users`, `block_users`, `unblock_users`: blocked users cannot message the number.

## Webhooks and events

- `list_webhooks`, `create_webhook`, `update_webhook`, `delete_webhook`: every event of the number goes to every webhook; the receiver filters by event type.
- `test_webhook_url` checks a URL before saving it; `create_webhook_catcher` and `get_webhook_catcher` give a temporary URL to see what the events look like.
- `get_direct_webhook`, `set_direct_webhook`, `remove_direct_webhook`: Meta sends the number's webhooks straight to the user's own URL instead of to app2email. While that is set, app2email no longer receives the number's incoming messages and statuses, so its inbox, e-mail forwarding and apps stop seeing them. Explain this and get a clear yes before setting it; `remove_direct_webhook` sends them back to app2email.

## QR codes, usage and analytics

- `list_qr_codes`, `create_qr_code`, `update_qr_code`, `delete_qr_code`: QR codes and links that open a chat with a prefilled message.
- `get_usage` gives the plan, the messages used and left, and the limits. `get_message_analytics`, `get_pricing_analytics` and `get_account_activity` give the figures over time.

## Apps

For Book-and-Pay, Tickets-and-Pay and Order-and-Pay settings, use the app-setup skill.
