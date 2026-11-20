---
name: send-messages
description: Send WhatsApp Business messages through app2email, as free text or as an approved template, to one person or a list. Use when the user asks to message, text, notify, remind or reply to someone on WhatsApp, to send a WhatsApp template, or to send a broadcast.
---

# Send WhatsApp messages with app2email

Use the tools of the **app2email** connector. Every tool works on the one WhatsApp number the user connected; there is no number argument.

## Before the first send

1. Call `check_key` once per conversation. It returns the number, the plan, the limits and what the connection may do. If it fails or the connection may only read, tell the user to reconnect the app2email connector with "read and send" or "everything", and stop.
2. If `check_key` says the connection is a sandbox, tell the user that nothing reaches WhatsApp.

## Pick free text or a template

WhatsApp allows free-form messages only within 24 hours of the contact's last message to the business.

- Call `get_conversation` for the contact and read `window_state`.
- **Window open:** use `send_text` for text, `send_media` for a picture, video, document or audio by link or uploaded media, `send_interactive` for buttons or a list, `send_location` for a place, `send_reaction` to react to a message.
- **Window closed, or a first message:** only an approved template can be sent. Call `list_templates`, pick an APPROVED template whose wording fits, and read what it needs with `get_template` (its placeholders, header media, buttons, and a complete example).
- Never rephrase a template's wording: only its placeholders take values.

## Send safely

Messages are real: they count against the business's plan, and Meta may charge the business for templates.

1. For a template, call `send_template` with `dry_run` set to true first. It sends nothing and returns the exact message for each recipient, or why a recipient would be skipped.
2. Show the user the recipients and the final text, and ask for a go-ahead, unless the user already told you exactly what to send to whom.
3. Send once, with an `idempotency_key` (any unique string, for example `send-<template>-<date>-<recipient>`). If a call times out, repeat it with the same key; never send again with a new key.
4. Report what happened in one or two lines: who it went to and the message id. Do not paste the whole tool answer.

Phone numbers are digits with the country code and no plus sign or spaces, for example `61412345678`. If the user gives a local number without a country code, ask which country.

## Many recipients

- `send_template` takes a list of numbers or a saved contact segment and creates a broadcast. Dry-run it first and give the user the count and a sample message.
- `list_broadcasts`, `get_broadcast` and `cancel_broadcast` follow and stop a broadcast.
- Marketing templates are not sent to numbers on the Do Not Call list; the dry run lists them as skipped.

## When a send is refused

The error tells you why and how to fix it (`how_to_fix`). Common ones:

- Outside the 24-hour window: switch to a template.
- Template not approved, paused or rejected: pick another one, or see the number-admin skill to fix or create one.
- Needs approval: the number has an approval gate. Tell the user; a person approves it in the dashboard or with `approve_send` (`list_approvals` shows what is waiting).
- Daily cap or plan limit reached: tell the user, do not retry.

## Rules

- When a contact asks to stop receiving messages, add them with `add_donotcall`.
- Never show access tokens or API keys in your output.
- Use `get_message` or `list_messages` to check delivery (`sent`, `delivered`, `read`, `failed` with the reason).
