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

Call `create_template` with the simple fields below. Do not build Meta-style `components`; the tool does that.

- **template_name**: lowercase letters, digits and underscores only, unique, descriptive, for example `order_shipped_v2`. Reusing an existing name fails.
- **category**: `MARKETING` for promotions, greetings and re-engagement, `UTILITY` for transactional updates the customer expects (orders, bookings, reminders), `AUTHENTICATION` for one-time codes.
- **language**: a locale code such as `en`, `en_US`, `hi`, `ar`.
- **body**: clear text, under 1024 characters, with `{{1}}`, `{{2}}` variables for the parts that change. Emojis are fine in the body.
- **body_examples**: one example value per variable, in order, for example `["Priya"]`. Required whenever the body has variables.
- **header_text** (optional): up to 60 characters, no variables, no emojis. Or one of **header_image_url**, **header_video_url**, **header_document_url** with a public https link.
- **footer** (optional): up to 60 characters.
- **buttons** (optional, up to 10): `{"type":"quick_reply","text":"Yes"}`, `{"type":"url","text":"Shop now","url":"https://..."}`, `{"type":"phone","text":"Call us","phone_number":"+91..."}`.
- **whatsappPhoneId** (optional): only needed when the account has more than one number; take it from `list_channels`.

Example:

```json
{
  "template_name": "diwali_greetings_2026",
  "category": "MARKETING",
  "language": "en",
  "header_text": "Happy Diwali",
  "body": "Dear {{1}}, wishing you and your family a Diwali full of light and joy.",
  "body_examples": ["Priya"],
  "footer": "With warm wishes from Selling Page"
}
```

If the tool returns `success: false`, read the message: it names the field to fix. Do not retry the same payload.

Write bodies that pass review: no misleading claims, no abusive content, variables that are not the whole message, and a greeting plus a clear purpose. For `UTILITY`, the text must relate to a transaction the customer already has.

## After creating

The template starts `PENDING`. Call `get_template_status` to sync and read the latest status. `APPROVED` can be sent; `REJECTED` includes Meta's reason, so fix the text with `update_template` and resubmit. Tell the user approval is Meta's decision and timing varies.

## Do not

- Do not send a template that is not `APPROVED`.
- Do not create templates for content the user did not ask for.
- Do not put personal data in the template text itself; use variables.
