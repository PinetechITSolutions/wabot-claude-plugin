---
name: whatsapp-templates
description: Create, review or check approval of WhatsApp message templates in Wabot. Use when the user wants a new template, asks why a template cannot be sent, wants to know a template's variables, or asks about Meta approval status.
---

# WhatsApp templates in Wabot

Templates are pre-approved message formats. Meta reviews every new or edited template before it can be sent, usually within minutes to a few hours.

## Look before you create

- `list_templates` with `status: "APPROVED"` shows what can be sent now. Prefer reusing an approved template over creating a near-duplicate.
- `get_template` returns the body, header, buttons and the number of `{{n}}` variables. Variables are positional: `{{1}}`, `{{2}}` and so on.

## Create a template

Call `get_account` for the `whatsappPhoneId`, then `create_template` with:

- **name**: lowercase letters, digits and underscores only, unique, descriptive, for example `order_shipped_v2`. Reusing an existing name fails.
- **category**: `MARKETING` for promotions and re-engagement, `UTILITY` for transactional updates the customer expects (orders, bookings, reminders), `AUTHENTICATION` for one-time codes.
- **language**: a locale code such as `en`, `en_US`, `hi`, `ar`.
- **body**: clear text, under 1024 characters, with `{{1}}`-style variables for the parts that change. Give an example value for every variable so Meta can review it.
- Optional header (text or image), footer, and buttons (quick replies, call-to-action URL or phone number).

Write bodies that pass review: no misleading claims, no abusive content, variables that are not the whole message, and a greeting plus a clear purpose. For `UTILITY`, the text must relate to a transaction the customer already has.

## After creating

The template starts `PENDING`. Call `get_template_status` to sync and read the latest status. `APPROVED` can be sent; `REJECTED` includes Meta's reason, so fix the text with `update_template` and resubmit. Tell the user approval is Meta's decision and timing varies.

## Do not

- Do not send a template that is not `APPROVED`.
- Do not create templates for content the user did not ask for.
- Do not put personal data in the template text itself; use variables.
