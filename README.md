# FlareMail plugin for Grok Build

Read, draft, and send mail from a [FlareMail](https://sendwithflare.com) account inside Grok Build.

FlareMail is a Cloudflare-native mailbox (custom domain, no IMAP/SMTP). This plugin points Grok at FlareMail's hosted MCP server so the agent can use the same inbox the FlareMail app does.

## Installation

In Grok Build, open `/marketplace`, search for **FlareMail**, and install.

On first connection Grok opens FlareMail's OAuth page. Sign in with the same email and password as the FlareMail app, then click **Allow access**. Google-only FlareMail accounts need a password set first.

You can also add the server by URL:

```bash
grok mcp add --transport http flaremail https://flaremail-api.tcrxx0.workers.dev/mcp
```

## Tools

| Tool | What it does |
|---|---|
| `whoami` | Authenticated user, account, onboarding status |
| `list_mailboxes` | Sender addresses on the account |
| `inbox_counts` | Unread totals and Drafts count |
| `list_emails` / `get_email` / `search_emails` | Read mail |
| `create_draft` / `send_draft` / `send_email` / `reply_to_email` / `schedule_email` | Write mail |
| `list_contacts` / `get_contact` / `import_contacts` / `import_drafts` | Directory and bulk drafts |
| `list_templates` / `update_email` | Templates and folder/read/star updates |

## Example prompts

```text
Draft a follow-up to last week's intro email and leave it in Drafts for me to review.
```

```text
Search my FlareMail inbox for messages from acme.com this week and summarize them.
```

```text
Send the saved draft about the invoice to billing@acme.com after I confirm the recipient.
```

## Authentication and security

This plugin connects only to FlareMail's hosted MCP endpoint:

`https://flaremail-api.tcrxx0.workers.dev/mcp`

There is no API key in the plugin config. Auth is OAuth 2.1 + PKCE (browser sign-in). The resulting token can read and send as the signed-in mailbox. Revoke access from FlareMail **Settings → Bots & MCP**.

Prefer `create_draft` until the user confirms recipients. Do not put secrets, API keys, or passwords in email bodies.

Setup walkthrough: [Connect an AI bot with MCP](https://sendwithflare.com/help/mcp-bots/).

## License

MIT
