---
name: conversations
description: Read, summarise and answer WhatsApp conversations of an app2email number, wait for a customer's reply, and hand a conversation to a person. Use when the user asks who wrote in, what a customer said, to catch up on the WhatsApp inbox, to reply to a customer, or to pass a chat to a human.
---

# WhatsApp conversations with app2email

Use the tools of the **app2email** connector. They work on the one WhatsApp number the user connected.

## Catch up

- `list_conversations` lists the contacts, most recent activity first; `window_open` says whether a free-form reply can be sent now. Use `since` to see only today's or this week's contacts.
- `get_conversation` gives one contact's conversation state: `window_state`, when the window closes, who owns it, the contact's name and the times of the last messages.
- `get_conversation_messages` gives the messages of one conversation, newest first. Fetch only as many as you need for the question.
- `get_message` gives one message and its delivery status.

When you summarise, give who wrote, when, what they want, and what is still unanswered. Quote customers only as far as needed.

## Answer

Replying is sending: follow the send-messages skill (free text inside the 24-hour window, a template outside it, and ask the user before sending unless they already told you what to send).

- `mark_read` marks a customer's message as read, so they see blue ticks.

## Wait for a reply

- `wait_for_message` waits up to 25 seconds for messages sent to the number, optionally only from one contact (`from`). Call it again to keep waiting; tell the user if nothing came.
- `list_events` returns the number's events (incoming messages, delivery statuses, opt-outs and the rest) after a cursor, so you can pick up where you left off. The first call switches the number's event feed on; earlier events are not kept.

## Hand over to a person

When a contact asks for a person, is upset, or asks something you cannot answer, call `set_conversation_owner` with `owner` set to `human`, a short reason and a summary of the conversation. The automatic replies stop for that contact, and the person taking over sees the reason and summary in the handoff e-mail and the dashboard. `until_minutes` gives the conversation back to the default after a while. Change the owner back only when the user asks.

## Privacy

- Conversations hold customers' personal data. Show only what the user asked for, and do not copy conversations into other tools or places unless the user asks.
- If a customer asks for their booking data to be deleted, tell the user. `erase_customer_data` cancels their future appointments without a message and anonymises their booking history; it cannot be undone, so confirm first.
