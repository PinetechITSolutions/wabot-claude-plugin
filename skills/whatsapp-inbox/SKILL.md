---
name: whatsapp-inbox
description: Read and summarise WhatsApp conversations from Wabot, find what customers said, check whether a chat is still inside the 24-hour reply window, and watch for new replies. Use when the user asks what a customer said, wants a summary of recent chats, or wants to be told when someone replies.
---

# Read WhatsApp conversations in Wabot

Conversations belong to the signed-in Wabot account. Only read what the user asks about, and never paste long raw chat dumps when a summary answers the question.

## Find the conversation

- `list_recent_chats` lists the most recent conversations with the contact and last activity. Page through with `page` and `pageSize` when needed.
- `find_contact` by phone gives the `contactId` for a specific customer.
- `get_messages` returns the history for a contact, newest last. Use `since` to read only what is new.

## Answer common questions

- **"What did X say?"**: quote the customer's latest inbound messages briefly, with timestamps.
- **"Summarise recent chats"**: group by contact, one line each: who, when, what they want, whether a reply is pending.
- **"Can I still reply?"**: the 24-hour window is open if the newest inbound message is less than 24 hours old. Say when it closes. If closed, point the user to an approved template (see the `whatsapp-outreach` skill).
- **"Who is waiting on us?"**: conversations whose last message is inbound and unanswered.

## Live replies

- `subscribe_replies`, optionally filtered by `phone` or `contactId`, pushes new inbound messages into this session as they arrive. Use it when the user wants to be told the moment someone answers.
- `wait_for_reply` waits up to 30 seconds for one reply after a send.
- `unsubscribe_replies` and `list_reply_subscriptions` manage subscriptions.

## Privacy

Treat message content as the customer's personal data: do not copy it anywhere outside the conversation, and do not infer sensitive attributes about customers.
