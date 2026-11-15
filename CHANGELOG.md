# Changelog

## 1.2.0 (2026-11-15)

- The Claude apps (claude.ai, Claude Desktop, and mobile) and ChatGPT connect by signing in to SigPen. No API key needed.
- A client without a key is asked to sign in, including to list tools.
- `get_account` says whether the assistant is connected with a key or a sign-in.
- Connected apps are listed in the developer dashboard, each with Disconnect.

## 1.1.0 (2026-11-14)

- Clients can connect and list tools without a key. Tool calls still need one.
- Claude Code and Cursor plugins, with a `send-for-signature` skill.

## 1.0.0 (2026-11-14)

- 14 tools: look up your account, documents, and templates; draft from text, a template, or your own PDF; place and fill in fields; set recipients; and ask you to approve the send.
