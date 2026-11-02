# Wabot WhatsApp plugin for Claude

Send WhatsApp messages, manage approved templates and contacts, and read customer conversations on your [Wabot](https://wabot.shop) account directly from Claude. The plugin connects Claude to the Wabot MCP server at `https://api.wabot.shop/mcp` and adds skills that teach Claude how WhatsApp messaging works: approved templates, the 24-hour customer-care window, template approval by Meta, and reading conversations responsibly.

Wabot is a WhatsApp Business Platform tool by Pinetech IT Solutions. You need a Wabot account with a connected WhatsApp Business number to use this plugin.

## What Claude can do with it

| Area | Tools |
| --- | --- |
| Account | `get_account`, `list_channels` |
| Templates | `list_templates`, `get_template`, `create_template`, `get_template_status`, `update_template` |
| Contacts | `find_contact`, `create_contact` |
| Messaging | `send_template_message`, `send_session_message` |
| Conversations | `list_recent_chats`, `get_messages`, `wait_for_reply`, `subscribe_replies`, `unsubscribe_replies`, `list_reply_subscriptions` |

Read-only tools run without a confirmation. Tools that send messages or change data ask for confirmation in Claude, and the skills tell Claude to show you the recipient and the exact text before any send.

## Install

**From the Claude directory (claude.ai, Claude Desktop, Cowork, Claude Code).** Open the directory, find "Wabot WhatsApp", add it, and sign in with your Wabot email and password when Claude asks. The same install syncs to Claude Code as `wabot@synced`.

**In Claude Code from this repository.**

```
claude plugin marketplace add PinetechITSolutions/wabot-claude-plugin
claude plugin install wabot@pinetech-wabot
```

Then run `/mcp` in a Claude Code session and sign in to Wabot when prompted.

**Without a marketplace.** Clone this repository and start Claude Code with `claude --plugin-dir ./wabot-claude-plugin`.

## Sign in

The first time Claude uses a Wabot tool it opens a Wabot sign-in page on `api.wabot.shop`. Enter the email and password you use at `app.wabot.shop`, pick the workspace Claude may use if you have more than one, and approve. Claude receives a token for that workspace only; your password is never shared with Claude or Anthropic. You can disconnect at any time from the connector settings in Claude.

Cursor, Codex and other MCP clients can also connect with the Client ID and Client Secret from Wabot under Account, Developer, Client Settings. See the [documentation](https://docs.wabot.shop/integrations/claude).

## Example prompts

- "Find the contact with phone 919400294110 and send them the approved template `order_shipped` with the tracking link https://example.com/track/123."
- "List recent WhatsApp chats and tell me which customers are waiting for a reply."
- "Create a utility template called `appointment_reminder` that reminds the customer of their appointment time and location, then check whether Meta approved it."
- "Did Anita reply after our last message? If the 24-hour window is still open, send her a thank-you message."

## Skills

- `whatsapp-outreach`: choosing between template and session messages, resolving contacts, confirming before sending, handling the 24-hour window.
- `whatsapp-templates`: template naming, categories, variables, Meta approval and rejections.
- `whatsapp-inbox`: finding and summarising conversations, checking the reply window, live reply subscriptions.

## Data handling and privacy

- The plugin contains no code that runs on your machine. It declares one remote MCP server, `https://api.wabot.shop/mcp`, operated by Pinetech IT Solutions, and three skills (plain Markdown).
- Claude sends tool calls to that server: the arguments you ask for (phone numbers, template names, message text) and the OAuth token for your Wabot workspace. The server answers with data from your own Wabot account. Nothing is sent to any other destination.
- Wabot stores messages, contacts and templates as part of your account, as described in the [Wabot privacy policy](https://wabot.shop/privacy-policy-2/). WhatsApp delivery is performed by Meta's WhatsApp Cloud API.
- Wabot keeps an audit log of tool calls (tool name, account, time) for security. It does not store your Claude conversation.
- Terms: [https://wabot.shop/terms-conditions/](https://wabot.shop/terms-conditions/).

## Privacy Policy

This plugin is governed by the Wabot privacy policy at [https://wabot.shop/privacy-policy-2/](https://wabot.shop/privacy-policy-2/). Data collected: the tool arguments you ask Claude to send and the results returned from your Wabot account. Usage and storage: processed by the Wabot API to perform the requested action; messages, contacts and templates are stored in your Wabot account. Third-party sharing: WhatsApp messages are delivered through Meta's WhatsApp Cloud API; no other third parties receive data. Retention: as set by your Wabot account and plan; OAuth tokens expire after one hour and refresh tokens after 60 days or when you disconnect. Contact: hello@wabot.shop.

## Support

Email [hello@wabot.shop](mailto:hello@wabot.shop) or read the [Wabot help center](https://docs.wabot.shop). Issues with this plugin can be opened in this repository.

## License

MIT. See [LICENSE](./LICENSE).
