# Maindex Expert

**Full-fidelity memory for AI agents who demand precision.**

Maindex Expert is the complete MCP interface — 14 tools and 6 resources that give AI agents direct, granular control over a persistent knowledge graph. Typed associations, collections, bulk operations, semantic search, supersession chains, and more.

When asked, 10 out of 10 LLMs said they prefer the Expert interface over Smart because of the additional control it gives them.

Both Smart and Expert services are included with every Maindex plan at no additional cost. They read and write the same memory graph, and you can switch between them or use both simultaneously.

**MCP Endpoint:** `https://expert.maindex.io/mcp`

---

## Quick Start

### 1. Connect your agent

Add the Maindex Expert MCP server to your AI platform of choice:

| Platform | How to connect |
|---|---|
| **Cursor** | Clone and install [cursor-expert-plugin](https://github.com/maindexapp/cursor-expert-plugin) |
| **Claude** | Clone and install [claude-expert-plugin](https://github.com/maindexapp/claude-expert-plugin) |
| **Gemini** | Install [gemini-expert-plugin](https://github.com/maindexapp/gemini-expert-plugin) as a Gemini Extension |
| **OpenClaw** | Install [openclaw-expert-plugin](https://github.com/maindexapp/openclaw-expert-plugin) |
| **Any MCP client** | Point your MCP client at `https://expert.maindex.io/mcp` (Streamable HTTP transport, OAuth 2.1) |

### 2. Authenticate

On first connection, you'll be redirected to [maindex.io](https://maindex.io) to sign in and authorize. OAuth handles the rest.

### 3. Start building your knowledge graph

Expert gives your agent fine-grained control. Create memories with rich metadata, build typed associations between them, organize with collections, search with full-text + semantic + hybrid retrieval, and manage everything with bulk operations.

---

## Tools

### Memory Management

| Tool | Description |
|---|---|
| `memory.keep` | Create a new memory with headline, body, kind, tags, collections, confidence, inline links, and more. |
| `memory.update` | Revise an existing memory (modes: body_append, body_replace, headline_replace, headline_and_body_replace, revision_only). Full history preserved. |
| `memory.forget` | Soft-delete a memory (restorable). Idempotent. |
| `memory.supersede` | Atomic replace — creates the new memory, marks the old one deprecated, creates a supersedes link, inherits tags. |
| `memory.bulk_keep` | Batch create up to 100 memories with shared defaults. Auto-links batch members. |
| `memory.bulk_update` | Batch operations: add/remove tags, set kind/canon_status/verification_status, manage collection membership, merge metadata, add links, or soft-delete. |

### Retrieval

| Tool | Description |
|---|---|
| `memory.search` | Full-text and semantic search. Strategies: auto, lexical, semantic, hybrid. Filters by tags, kind, canon_status, collection, confidence range, verification_status. Score breakdowns and graph neighbor expansion available. |
| `memory.list` | Structured filter and browse. Same filter set as search but no free-text query — use for browsing by criteria. |
| `memory.recall` | Get a single memory by UUID or short ID (e.g. `mem-1jc4`). Optionally include revisions and links. |

### Graph & Organization

| Tool | Description |
|---|---|
| `memory.associate` | Create typed links between memories. Relation types: supports, contradicts, depends_on, expands, derived_from, example_of, belongs_to, alternative_to, supersedes, and custom. Inverse links auto-created. |
| `memory.get_related` | Discover related memories via links, shared tags, or collection membership. |
| `collection.manage` | Multi-action tool: create, update, delete, add_members, remove_members, list, get. Collections support nesting, icons, colors, and descriptions. |
| `collection.unlock` | Unlock passphrase-protected collections for the current session. |

### System

| Tool | Description |
|---|---|
| `system.report_bug` | Report bugs directly to the Maindex team with severity, category, reproduction steps, and context. |

---

## Resources

MCP resources provide read-only context your agent can access at any time:

| Resource URI | What it provides |
|---|---|
| `memory://tags` | All tags across your account |
| `memory://collections` | All collections with hierarchy and member counts |
| `memory://recent` | Most recently updated memories |
| `memory://relation-types` | Available relation types with descriptions and inverses |
| `memory://sessions` | Recent interaction sessions |

---

## Authentication

Maindex uses **OAuth 2.1** for all connections. MCP clients handle the OAuth flow automatically — on first use, a browser window opens for sign-in and authorization.

For headless environments, API keys with per-key revocation are available from your [dashboard](https://maindex.io/dashboard).

---

## Key Concepts

### Tagging

Use faceted tags for structured categorization: `domain:physics`, `project:my-app`, `function:premise`, `status:blocked`, `topic:authentication`. Keep tags lowercase and hyphenated.

### Canon Status

Controls how much weight a memory carries:

| Status | When to use |
|---|---|
| `draft` | Work-in-progress, unvalidated |
| `proposed` | Awaiting review or confirmation |
| `accepted` | Confirmed knowledge, verified decisions |
| `deprecated` | Outdated, superseded |
| `alternative` | Valid but not chosen |
| `meta` | Preferences, workflow notes, agent config |

### Memory Kinds

`note`, `fact`, `idea`, `decision`, `constraint`, `question`, `summary`, `artifact`, `task_context`

### Short IDs

Every memory has both a UUID and a short ID (e.g. `mem-1jc4`). Short IDs are human-readable and token-efficient. Both formats are accepted everywhere.

### Superseding

When a fact or decision changes, use `memory.supersede` rather than delete-and-recreate. The old memory is marked deprecated with a `superseded_by` pointer, and a `supersedes` link is created automatically — preserving the full history chain.

---

## FAQ

### Why not use my LLM's built-in memory?

Built-in LLM memory is typically a flat list of text notes with no structure, no search beyond keyword matching, and no way to export or move your data to another service. If you switch providers, your memories stay behind.

Maindex gives you a structured, searchable memory graph accessible through both an agent-facing MCP interface and a human-facing dashboard. Your data is portable across every platform that supports MCP.

### Should I use Smart or Expert?

Both are included, both read and write the same memory graph, and you can switch at any time. Use **Expert** when the agent needs direct, granular control. Use **Smart** when you want the agent to send high-level intents and let the pipeline decide.

See the [Smart service](https://github.com/maindexapp/smart-service) for the streamlined 4-tool API.

### What are synapses?

A synapse is a single unit of memory usage. Every time an agent or API call reads from or writes to your memory graph, the processing is measured in synapses. Your plan includes a generous monthly quota that resets each billing period. Check your usage on the [dashboard](https://maindex.io/dashboard).

### What are locked collections?

A locked collection is protected by a passphrase. Memories inside are hidden from all sessions and API calls until explicitly unlocked with `collection.unlock`. This lets you compartmentalize information (work vs personal, private notes, client data) without exposing it to every agent session.

### Can I export my data?

Yes. Your dashboard has three export options:
- **Full Export** (JSON): memories, tags, collections, links, revision history, conversations — everything
- **Memories JSON**: clean memory-only JSON
- **Memories CSV**: spreadsheet-friendly format

You can also import a Full Export JSON back into a new account.

### Is my data private?

Every account is a fully isolated tenant. Your data is never shared, mixed, or accessible by other users. All data is encrypted at rest (AES-256) and in transit (TLS 1.2+). Maindex does not train models on your data.

---

## Plugin Repos

| Platform | Plugin |
|---|---|
| Cursor | [cursor-expert-plugin](https://github.com/maindexapp/cursor-expert-plugin) |
| Claude | [claude-expert-plugin](https://github.com/maindexapp/claude-expert-plugin) |
| Gemini | [gemini-expert-plugin](https://github.com/maindexapp/gemini-expert-plugin) |
| OpenClaw | [openclaw-expert-plugin](https://github.com/maindexapp/openclaw-expert-plugin) |

## See Also

- [Smart Service](https://github.com/maindexapp/smart-service) — Streamlined 4-tool interface
- [maindex.io](https://maindex.io) — Dashboard and account management
- [Documentation](https://docs.maindex.io)
