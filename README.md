# app2email for Claude

![app2email](icon.png)

Let Claude send and receive WhatsApp Business messages and look after your WhatsApp number through [app2email](https://www.app2email.com).

app2email connects a business's own WhatsApp number (the official WhatsApp Business Cloud API: the business owns the number and pays Meta directly) to e-mail, to automations and to AI agents. This plugin gives Claude the app2email connector plus four skills that tell it how to use the connector safely.

## What you can ask Claude

- **Send messages.** "Send the order_ready template to 61412345678 with order number 1042." "Reply to Ava's last message and say we open at 9." Claude sends free text inside WhatsApp's 24-hour reply window and an approved template outside it, always does a dry run first and asks you before anything is sent, unless you already told it to send.
- **Read and answer conversations.** "Who wrote to us today?" "Summarise the conversation with 61412345678." "Hand this one to a person." Claude can wait for a reply and pass a conversation to a member of your team.
- **Manage the number.** Templates (create, edit, check why one was rejected), the WhatsApp business profile, contacts and the Do Not Call list, webhooks, QR codes, number health and usage.
- **Set up the apps.** Book-and-Pay (bookings), Tickets-and-Pay (event tickets and seat plans) and Order-and-Pay (a shop on WhatsApp): change their settings, export the whole setup as a file, check a changed file, apply it and undo it.

## Getting started

1. You need an app2email account with a connected WhatsApp number and a plan that includes the API. Sign up at [app2email.com](https://www.app2email.com/register).
2. Add this plugin, then connect the **app2email** connector from the plugin's Connectors tab (in Claude Code, run `/mcp` and sign in).
3. Sign in to app2email in the window that opens, choose the WhatsApp number Claude may use and what it may do: **read**, **read and send**, or **everything** (needed for templates, settings and the apps). Tick sandbox to try it first: nothing is then sent to WhatsApp.
4. Ask Claude "check my app2email connection" to see the number, plan and limits.

One connection works on one WhatsApp number. To work on another number, disconnect and connect again, choosing the other number. You can see and revoke the connection at any time on the API keys page of your app2email dashboard, where it is listed with a name starting with "OAuth:".

## What the plugin runs and what data it sends

- The plugin is only Markdown skills and one connector address. It has no scripts, hooks or local programs, and it stores nothing on your computer.
- The connector is app2email's hosted MCP server at `https://api.app2email.com/mcp`. When Claude uses a tool, the tool's arguments (for example the recipient's WhatsApp number and the message text) are sent there, over HTTPS, with the access token from your sign-in.
- app2email sends your messages to WhatsApp through Meta's WhatsApp Business Cloud API, keeps the message history of your number as described in its [privacy policy](https://www.app2email.com/legal/privacy-policy), and forwards incoming messages to your e-mail and webhooks as you configured them in app2email.
- Sending a message is real: it counts against your app2email plan and Meta may charge your business its own fee for templates. The tools mark which actions only read and which change or send something, so Claude asks before the ones that send or change.

## Links

- Connector and tool reference: https://www.app2email.com/docs/api/mcp
- REST API documentation: https://www.app2email.com/docs/api
- Support: support@app2email.com, or https://www.app2email.com/dashboard/support when signed in
- Privacy policy: https://www.app2email.com/legal/privacy-policy
- Terms of service: https://www.app2email.com/legal/terms-of-service

WhatsApp is a trademark of Meta Platforms, Inc. app2email is an independent service and is not affiliated with or endorsed by Meta.
