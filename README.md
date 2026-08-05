# Maindex Expert MCP Server

This is the official Maindex Expert MCP server published by [Maindex](https://maindex.io).

Maindex Expert is a hosted remote MCP service. No local installation is required.

[Website](https://maindex.io) | [Help & FAQ](https://maindex.io/help) | [Dashboard](https://maindex.io/dashboard)

Maindex Expert is the full-control MCP interface for capable agents and advanced workflows. It exposes structured memory, search, filtering, relationships, collections, revisions, supersession, and bulk maintenance operations — 15 tools and 5 resources that give AI agents direct, granular control over a persistent knowledge graph.

Both Smart and Expert services are included with every Maindex plan at no additional cost. They read and write the same memory graph, and you can switch between them or use both simultaneously.

## Endpoint

`https://expert.maindex.io/mcp`

## Tools

- `memory_keep` — create a structured memory entry.
- `memory_update` — update an existing memory and create a revision.
- `memory_forget` — soft-delete a memory.
- `memory_recall` — retrieve a single memory by ID.
- `memory_list` — list memories using structured filters.
- `memory_search` — search memories using lexical, semantic, or hybrid retrieval.
- `memory_associate` — create typed links between memories.
- `memory_unassociate` — remove typed links between memories (by link ID or memory triple).
- `memory_get_related` — discover related memories by link, tag, or collection.
- `memory_supersede` — replace an existing memory with a new canonical entry.
- `memory_bulk_keep` — create multiple memories in one call.
- `memory_bulk_update` — batch-update multiple memories.
- `collection_manage` — create, update, delete, list, and manage memory collections.
- `collection_unlock` — unlock locked memory collections for the session using a passphrase.
- `system_report_bug` — report a bug to Maindex maintainers.

## Installation

Use the hosted endpoint in any MCP client that supports remote streamable HTTP MCP servers.

```json
{
  "mcpServers": {
    "maindex-expert": {
      "url": "https://expert.maindex.io/mcp"
    }
  }
}
```

Or install a platform-specific plugin:

| Platform | How to connect |
|---|---|
| **Cursor** | Clone and install [cursor-expert-plugin](https://github.com/maindexapp/cursor-expert-plugin) |
| **Claude** | Clone and install [claude-expert-plugin](https://github.com/maindexapp/claude-expert-plugin) |
| **Gemini** | Install [gemini-expert-plugin](https://github.com/maindexapp/gemini-expert-plugin) as a Gemini Extension |
| **OpenClaw** | Install [openclaw-expert-plugin](https://github.com/maindexapp/openclaw-expert-plugin) |

## Example Usage

- `Search my memories about the Smart MCP rollout.`
- `Create a memory and tag it project:maindex.`
- `Link these two memories because one supports the other.`
- `Show the revision history for mem-abc123.`
- `Supersede the old pricing decision with the new accepted one.`

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

When a fact or decision changes, use `memory_supersede` rather than delete-and-recreate. The old memory is marked deprecated with a `superseded_by` pointer, and a `supersedes` link is created automatically — preserving the full history chain.

---

## Teams Workspaces

Team workspaces let multiple members share a scoped memory graph within your account. Expert tools support team context on reads and writes:

### Team filter on read tools

Optional `team` parameter on `memory_search`, `memory_recall`, `memory_list`, and `collection_manage` (list/get):

- **Omit** — mixed personal + team results (default)
- **`personal`** — personal memories only
- **Team slug** — limit results to that team workspace

### Team writes

- **`memory_keep`** — set both `team` and `collection` to create a memory in a team collection. The personal `collections[]` array remains for personal multi-collection writes.
- **`collection_manage`** — use action `create_team_collection` (with `team` and collection name/slug) to create a collection inside a team workspace.

Other mutation tools (`memory_update`, `memory_forget`, etc.) operate on existing memories by ID; use team-namespaced IDs when targeting team-scoped memories.

### Team-namespaced IDs

Memories created in a team workspace use namespaced short IDs: `team-slug:mem-xxxx` (e.g. `acme:mem-j4`). Both bare `mem-xxxx` and namespaced forms are accepted where memory IDs are referenced. Team collection slugs may appear as `team-slug:collection-slug`.

---

## FAQ

### Why not use my LLM's built-in memory?

Built-in LLM memory is typically a flat list of text notes with no structure, no search beyond keyword matching, and no way to export or move your data to another service. If you switch providers, your memories stay behind.

Maindex gives you a structured, searchable memory graph accessible through both an agent-facing MCP interface and a human-facing dashboard. Your data is portable across every platform that supports MCP.

### Should I use Smart or Expert?

Both are included, both read and write the same memory graph, and you can switch at any time. Smart is optimized for simple user-controlled memory. Expert is optimized for capable agents that need direct control over the full memory graph.

For a smaller, safety-constrained interface, use Maindex Smart at `https://maindex.io/mcp`. See the [Smart service](https://github.com/maindexapp/smart-service) for the streamlined 4-tool API.

### What are synapses?

A synapse is a single unit of memory usage. Every time an agent or API call reads from or writes to your memory graph, the processing is measured in synapses. Your plan includes a generous monthly quota that resets each billing period. Check your usage on the [dashboard](https://maindex.io/dashboard).

### What are locked collections?

A locked collection is protected by a passphrase. Memories inside are hidden from all sessions and API calls until explicitly unlocked with `collection_unlock`. This lets you compartmentalize information (work vs personal, private notes, client data) without exposing it to every agent session.

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
- [Help & FAQ](https://maindex.io/help)

## License

MIT
