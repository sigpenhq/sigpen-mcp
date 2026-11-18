# Changelog

## 1.3.0 (2026-11-18)

- SigPen is in Claude's connector directory. In Claude, open Customize, then Connectors, and search for SigPen.
- Tools carry their titles in their annotations, so clients and directories show readable names.
- `create_document_from_template` works with `role_assignments` on SigPen's library templates: each role gets the fields whose label starts with it.
- Library templates put their fields where the PDF draws them, with the printed labels and real signature and date fields. The NDA's roles are now "Party 1" and "Party 2".
- `get_document` shows each draft field's signer, and `get_template` shows the roles of library templates.
- `list_templates` includes descriptions, so templates with similar names can be told apart.
- `get_account` calls its monthly count `created`, since drafts count.
- When a send can't be approved, the approval page and the error say what to fix, like which fields have no recipient.
- `delete_draft` also covers drafts made from an upload.

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
