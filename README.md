# FlareMail plugin for Grok Build

Read, draft, and send mail from a [FlareMail](https://sendwithflare.com) account inside Grok Build. Bots can also list domains, start a new sending+receiving domain, and create aliases on a verified hostname.

FlareMail is a Cloudflare-native mailbox (custom domain, no IMAP/SMTP). This plugin points Grok at FlareMail's hosted MCP server so the agent can use the same inbox the FlareMail app does.

## Installation

In Grok Build, open `/marketplace`, search for **FlareMail**, and install.

On first connection Grok opens FlareMail's OAuth page. Sign in with the same email and password as the FlareMail app, then click **Allow access**. Google-only FlareMail accounts need a password set first.

You can also add the server by URL:

```bash
grok mcp add --transport http flaremail https://flaremail-api.tcrxx0.workers.dev/mcp
```

## Tools

Hosted at `https://flaremail-api.tcrxx0.workers.dev/mcp`.

| Tool | What it does |
|---|---|
| `whoami` | Authenticated user, account, onboarding status |
| `list_mailboxes` | Sender addresses on the account |
| `inbox_counts` | Unread totals and Drafts count |
| `list_emails` / `get_email` / `search_emails` | Read mail |
| `create_draft` / `send_draft` / `send_email` / `reply_to_email` / `schedule_email` | Write mail |
| `list_contacts` / `get_contact` / `import_contacts` / `import_drafts` | Directory and bulk drafts |
| `list_templates` / `update_email` | Templates and folder/read/star updates |
| `list_domains` | Sending/receiving domains, mailboxes on each, and whether the next extra domain needs Paddle |
| `add_domain` | Start configuring a sending+receiving domain (optional `local_part`). Never charges — returns `requires_payment` + `checkout_url` when a paid seat is needed |
| `create_mailbox` | Create an alias/mailbox like `hello@winos.lol` on a verified domain |
| `get_domain_setup` | DNS / Email Routing status; `refresh=true` re-checks Cloudflare |

Bots should call `create_draft` (or `import_drafts`) and wait for the user to confirm recipients before `send_draft` / `send_email`. Domain, billing, and DNS handoffs return `next_steps` for a human — Paddle checkout, leftover MX, or a Cloudflare reconnect. Show those steps; do not treat them as a silent success.

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

```text
List my FlareMail domains, then create hello@winos.lol on the existing winos.lol domain.
```

```text
Add winos.lol as a sending domain. If payment or DNS is needed, show me the next_steps — do not charge anything.
```

## Authentication and security

This plugin connects only to FlareMail's hosted MCP endpoint:

`https://flaremail-api.tcrxx0.workers.dev/mcp`

There is no API key in the plugin config. Auth is OAuth 2.1 + PKCE (browser sign-in). The resulting token can read and send as the signed-in mailbox. Revoke access from FlareMail **Settings → Bots & MCP**.

Prefer `create_draft` until the user confirms recipients. Do not put secrets, API keys, or passwords in email bodies.

Setup walkthrough: [Connect an AI bot with MCP](https://sendwithflare.com/help/mcp-bots/).

## License

MIT
