---
name: whatsapp-outreach
description: Send a WhatsApp message to a customer through Wabot. Use when the user wants to message, notify, follow up with, or reach a customer or lead on WhatsApp, send an approved template, or reply inside the 24-hour window.
---

# Send WhatsApp messages through Wabot

Use the Wabot MCP tools. Never guess a phone number, template name or WhatsApp number: look each one up first.

## Before the first send in a conversation

1. Call `get_account` and note the enabled `whatsappPhoneId` values. If there are several, call `list_channels` and ask the user which number to send from, unless they already said.
2. Resolve the recipient with `find_contact` by phone (digits with country code, no plus sign, for example `919400294110`). If there is no contact and the user gave a name and number, call `create_contact`.

## Choose the message type

WhatsApp allows free-form text only inside the 24-hour customer-care window that opens when the customer last wrote in. Outside it, only an approved template can be sent.

- **Template message** (`send_template_message`): first contact, re-engagement, notifications, anything when you are not sure the window is open. Pick an `APPROVED` template from `list_templates`, read it with `get_template` to learn its variables, and fill every variable. Pass `mediaUrl` for image-header templates.
- **Session message** (`send_session_message`): a reply while the window is open. Check `get_messages` for the contact first: an inbound message in the last 24 hours means the window is open. If the send fails with a window error, fall back to a template and tell the user why.

## Confirm before sending

Sending is irreversible. Show the user the recipient, the sending number, and the exact text (with variables filled) and get a yes before calling the send tool. Send one message per request unless the user asked for a batch, and never send marketing templates to people who did not opt in.

## After sending

- Report the result briefly: recipient, template or text, and the message id.
- If the user wants to know the answer, call `wait_for_reply` (up to 30 seconds) or `subscribe_replies` for live replies, then summarise what came back.

## Common errors

- `required_grant`: the account's API credential lacks that permission. Tell the user to enable it in Wabot under Account, Developer.
- Template `PENDING` or `REJECTED`: it cannot be sent. Offer another approved template or see the `whatsapp-templates` skill.
- Window closed: switch to a template.
