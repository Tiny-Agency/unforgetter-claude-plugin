# Unforgetter

Unforgetter keeps project context, decisions and preferences available to the AI apps you connect. Say it once while you work: your AI app saves it automatically, without a separate save request. Then pick up where you left off in a new conversation or in another app.

This repository contains the Unforgetter plugin for Claude. It holds one skill (`memory`), an icon and this README. It has no hooks and no MCP server of its own, runs no code, and makes no network calls. The skill is written guidance for the AI app. It covers when to load project context, how to show sources, and when to save a decision, preference, correction or open thread you state. It tells the app to save these automatically during ordinary work, without asking each time, once you have allowed saving when you connect.

The Unforgetter tools come from the hosted Unforgetter service at `https://app.unforgetter.com/mcp`. You need an Unforgetter account. Sign-up is open at https://app.unforgetter.com/sign-up. You must be 13 or older, and under 18 you need a parent's or guardian's consent (see the terms).

## Connect Claude

1. In Claude, open **Settings → Connectors → Add custom connector** and enter `https://app.unforgetter.com/mcp`. Choose **Connect**, then sign in with your Unforgetter account and review the permissions.
2. Download the Claude plugin ZIP from https://app.unforgetter.com/connect?app=claude. Then open **Customize → Plugins → Add → Upload plugin**. The plugin's tools come from the connector, so don't add Unforgetter a second time.
3. In a conversation, choose **+ → Plugins → Unforgetter → memory**.

## Permissions and automatic saving

When you connect, the app asks for these permissions. You allow or deny them together:

- **Read memory** (`memory:read`). Gives the app the 8 read tools.
- **Save updates** (`memory:propose`). Adds the 4 write tools. It is requested by default, so the app can save to your memory once you allow the connection.

With the save permission, consent is given once, at connect. After that, the app sends the decisions, preferences, corrections and open threads you state to Unforgetter as you work, without asking each time. That is what lets the memory keep up without extra steps. To stop it, tell the app to stop saving in a conversation, or revoke the app's access at https://app.unforgetter.com/account. After each save, the app ends its reply with a short "Saved to Unforgetter" line, so you can see what was saved.

## The 12 tools

Read tools. They only read, and change nothing:

| Tool | What it does |
|---|---|
| `get_context` | Gives a short briefing for the task at hand: current decisions, preferences, open questions and context, with sources. |
| `search_memory` | Searches your saved memory for more detail, optionally continuing from a briefing. |
| `get_document` | Opens one memory document by its path, so you can see the source of an answer. |
| `list_documents` | Lists one page of your memory documents: path, title, kind and whether each is current. |
| `search_source_documents` | Searches documents you imported. These are dated reference material, not current memory. |
| `get_connection_info` | Shows which account, collections and permissions this connection has. |
| `get_memory_status` | Shows how many updates are waiting, any failures, and when memory was last published. |
| `get_proposal_status` | Shows what happened to a proposed update: accepted, deferred, superseded, not eligible or not found. It returns the outcome, not the text. |

Write tools. They need the update permission. Only `delete_memory` removes saved memory, and only when you ask:

| Tool | What it does |
|---|---|
| `propose_memory` | Sends a decision, constraint, preference, open question or correction you stated, in your own words. Called automatically during ordinary work. The app confirms it with a short "Saved to Unforgetter" line. |
| `propose_source_correction` | Proposes a correction to one quoted passage in a source. The original is kept as history. |
| `record_work_result` | Saves a short report of work an agent finished, with references to its evidence. The evidence is recorded as reported; Unforgetter does not check it. |
| `delete_memory` | Permanently deletes one item of your own personal memory when you ask: a saved memory, a pending update or a work report, together with the statement it came from. If other current items come from the same statement, the app shows them and asks before deleting them too. Team memory can't be deleted. It can't be undone. |

## How updates are processed

Unforgetter Cloud stores each update when it arrives and processes it automatically with OpenAI, after a short wait. There is no fixed processing time. Each one is either accepted as current memory, which can replace an older entry, or deferred with a reason. "Saved to Unforgetter" means an update was received and stored. It becomes current memory once accepted; a later lookup shows it with its source. When an account limit is reached, new updates are declined, and memory that is already saved stays readable.

## Try it

Run 1 and 2 in the same conversation. Run 3 in a new conversation once the update is accepted.

1. "Draft a two-line update for beta testers. We decided today that the beta stays invite-only."
   - The app writes the draft, saves the decision with `propose_memory` without being asked, and ends with a short "Saved to Unforgetter" line. The update is not current memory until it is accepted.
2. "Has my latest update been accepted into memory yet, or is it still waiting?"
   - The app calls `get_proposal_status` (or `get_memory_status`) and reports the real status.
3. "What have we decided about the beta? Show me where that comes from."
   - The app calls `get_context` (and maybe `get_document`) and answers with the source.

## What is sent where

- Selected parts of the conversation are sent to Unforgetter and to OpenAI: mainly the sentences where you state a decision, preference or correction, with their project context, and the short search text the app uses to look things up. Not the whole conversation.
- Unforgetter only receives what your AI app sends in a tool call. It does not read the AI app's own memory, chat history or files.
- Saved text is stored in Unforgetter Cloud and processed with OpenAI. OpenAI also creates the search embeddings for your memory and for each search query. Unforgetter does not store the search text itself.
- Reading memory sends relevant excerpts to the AI app you use, where that provider's terms apply.
- Hosted on Railway (EU region, Netherlands). OpenAI and other providers listed in the privacy policy also process data, some of them outside the EU.
- Keep passwords, API keys and sensitive personal information out of your memory.

## Revoke access, export and deletion

- At https://app.unforgetter.com/account you can revoke an app's access, or pause processing. While processing is paused, apps can still send updates. They are stored but not processed until you resume.
- Disconnecting only in the AI app may not revoke access in Unforgetter.
- Revoking access does not delete saved memory. Saved memory is kept until you delete it or close your account.
- To see what is saved, ask your AI app, for example "list my Unforgetter memories". To delete one, ask the app to delete it; this is permanent. There is no memory view on the website yet. To export your memory or close your account, email hello@unforgetter.com.

## Links

- Website: https://unforgetter.com
- Documentation: https://unforgetter.com/docs
- Privacy policy: https://unforgetter.com/privacy
- Terms: https://unforgetter.com/terms
- Security and ownership: https://unforgetter.com/security
- Support: https://unforgetter.com/support, or hello@unforgetter.com. Please don't send passwords or other secrets.

Unforgetter is made by Tiny Agency. More at unforgetter.com.

Publisher: Tiny Agency ApS, Denmark. Repository: https://github.com/pete-life/unforgetter-claude-plugin

## License

MIT. See the `LICENSE` file.
