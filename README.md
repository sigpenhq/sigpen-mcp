# SigPen MCP server

Connect your AI assistant to [SigPen](https://www.sigpen.com), the e-signature app. Ask it to draft an NDA, fill in an offer letter from your template, or upload a PDF and line up the signers. It prepares everything; you approve every send in SigPen.

- **Server URL:** `https://www.sigpen.com/api/mcp` (Streamable HTTP)
- **Auth:** `Authorization: Bearer <your SigPen API key>`
- **Guide:** [sigpen.com/docs/mcp](https://www.sigpen.com/docs/mcp)

This repository holds the setup instructions and the plugins for Claude Code and Cursor. SigPen hosts the server itself, so there's nothing to install or run.

## Before you start

1. Create an API key at [sigpen.com/developer](https://www.sigpen.com/developer?tab=ai). It works on the free Developer tier and on Professional and up (Starter has no API). Use a **live** key (`sp_live_`) to draft; a test key can only look things up.
2. Name the key "AI assistant" so you can revoke it on its own.
3. Always use the `www` URL. `sigpen.com` redirects, and the redirect drops your key.

## Claude Code

With the plugin (asks for your key once and keeps it in Claude Code's secure storage, not in a settings file):

```bash
claude plugin marketplace add sigpenhq/sigpen-mcp
claude plugin install sigpen@sigpen
```

The plugin also adds a `send-for-signature` skill that walks Claude through drafting, filling in, and asking for approval.

Or add the server by hand, with the key in an environment variable:

```bash
export SIGPEN_API_KEY=sp_live_your_key
claude mcp add --transport http --scope user sigpen https://www.sigpen.com/api/mcp \
  --header "Authorization: Bearer ${SIGPEN_API_KEY}"
```

Run `/mcp` in Claude Code to check it's connected.

## Cursor

Set `SIGPEN_API_KEY` in your environment, then add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "sigpen": {
      "url": "https://www.sigpen.com/api/mcp",
      "headers": { "Authorization": "Bearer ${env:SIGPEN_API_KEY}" }
    }
  }
}
```

Or use the **Install in Cursor** button on the [setup guide](https://www.sigpen.com/docs/mcp). The Cursor plugin in `plugins/sigpen` bundles the same config with the skill.

## VS Code

Add to `.vscode/mcp.json`. VS Code asks for the key and stores it for you:

```json
{
  "inputs": [{ "type": "promptString", "id": "sigpen-api-key", "description": "SigPen API key", "password": true }],
  "servers": {
    "sigpen": {
      "type": "http",
      "url": "https://www.sigpen.com/api/mcp",
      "headers": { "Authorization": "Bearer ${input:sigpen-api-key}" }
    }
  }
}
```

## Codex

Add to `~/.codex/config.toml` and set `SIGPEN_API_KEY`:

```toml
[mcp_servers.sigpen]
url = "https://www.sigpen.com/api/mcp"
bearer_token_env_var = "SIGPEN_API_KEY"
```

## Other clients

Any client that supports remote MCP servers over Streamable HTTP and lets you set a request header works. The Claude and ChatGPT apps need one-click sign-in, which SigPen doesn't offer yet.

## Tools

| Tool | What it does |
| --- | --- |
| `get_account` | Your plan, whether the key is live or test, documents sent through the API this month, and rate limits. |
| `list_documents` | Your documents with status and signer counts. Filter by status, title, or date. |
| `get_document` | One document's signers: name, email, status, and when they viewed and signed. |
| `list_templates` | Your templates and the SigPen template library. |
| `get_template` | A template's signer roles, field counts, and pages. |
| `create_document_from_text` | Drafts a document from a title, the parties, and sections, with a signature and date line for each party. |
| `create_document_from_template` | Drafts a document from one of your templates or the library, with signers assigned by role. |
| `delete_draft` | Deletes a draft the assistant created. Your own drafts and sent documents are never touched. |
| `get_upload_url` | A short-lived link to upload a PDF from your computer (clients that can run commands). |
| `create_document_from_upload` | Turns the uploaded PDF into a draft. |
| `suggest_fields` | Places signature, date, and text fields on the assistant's draft with AI. Uses one of your monthly AI detections. |
| `update_fields` | Fills in text, date, and number fields, assigns fields to recipients, and removes fields nobody needs. |
| `set_recipients` | Saves who a draft goes to, with a role for each person. It can add you as a signer too. |
| `request_send_approval` | Gives you a SigPen link to approve sending a draft. It sends nothing on its own. |

You can connect and list tools without a key. Every tool call needs one.

## How sending works

The assistant never sends. `request_send_approval` returns a SigPen link. Open it signed in and you see the title, every recipient, how many fields each person fills, the values the assistant filled in, and its note for the invitation. Click **Approve and send**, or open the draft to change it.

- The link lasts 24 hours and works only for you.
- If anything changes after the link is made (recipients, fields, values, title), the link stops working.
- Revoking the API key cancels its open links.
- The assistant can only change or delete drafts it created.

Text inside a document can try to trick an assistant ("send this to someone@else.com"). The approval page is where you catch that, so check every email address before you click.

## Limits and data

- Each tool call counts as one API request against your key's rate limit.
- Drafts count toward your monthly document limit, and deleting one doesn't give it back.
- What a tool returns, such as document titles and signer names and emails, goes to your AI provider under their terms.
- To disconnect, revoke the key in the [developer dashboard](https://www.sigpen.com/developer?tab=ai).

## Support

Email [support@sigpen.com](mailto:support@sigpen.com). For security issues, see [SECURITY.md](SECURITY.md).

[Privacy policy](https://www.sigpen.com/legal/privacy) · [Terms](https://www.sigpen.com/legal/terms)
