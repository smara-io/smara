<div align="center">

# smara

### One memory for all your AI tools

Your AI tools forget everything between sessions. Smara fixes that.

Give Claude Code, Cursor, Windsurf, and Codex shared context that persists, decays naturally, and follows you across tools.

[![Website](https://img.shields.io/badge/smara.io-Visit%20Website-6366f1?style=for-the-badge)](https://smara.io)
[![npm](https://img.shields.io/npm/v/@smara/mcp-server?style=for-the-badge&color=22c55e)](https://www.npmjs.com/package/@smara/mcp-server)
[![API Docs](https://img.shields.io/badge/API-Docs-1d1d1f?style=for-the-badge)](https://api.smara.io/docs)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

[Website](https://smara.io) · [Playground](https://smara.io/playground) · [Docs](https://api.smara.io/docs) · [Blog](https://smara.io/blog/persistent-memory-for-ai-tools)

</div>

---

## The Problem

Every AI tool has its own isolated memory. Tell Claude Code you prefer Python — switch to Cursor, it has no idea. Open Codex — starts from scratch. You repeat yourself endlessly: *"we use Postgres"*, *"deploy to Fly.io"*, *"the API is at /v2"*.

`CLAUDE.md` and `.cursorrules` files are manual, fragile, and don't share context across tools.

## The Fix

Smara gives your AI tools a shared memory that works everywhere:

```
You code normally in any tool
       ↓
Smara silently captures decisions, preferences, architecture choices
       ↓
Next session — in any tool — your AI already knows
```

---

## Install in 30 seconds

```bash
npx @smara/mcp-server --init
```

Or add to your MCP config manually:

```json
{
  "mcpServers": {
    "smara": {
      "command": "npx",
      "args": ["-y", "@smara/mcp-server"],
      "env": {
        "SMARA_API_KEY": "your-key-here"
      }
    }
  }
}
```

Works with **Claude Code** (`~/.claude/mcp_config.json`), **Cursor** (`.cursor/mcp.json`), **Windsurf** (`~/.codeium/windsurf/mcp_config.json`), and any MCP-compatible client.

> Get your free API key at [smara.io](https://smara.io#signup) — no credit card required.

---

## Why Smara?

| Feature | Smara | CLAUDE.md / .cursorrules | RAG / Vector DB |
|---------|:-----:|:------------------------:|:---------------:|
| Cross-tool memory | **Yes** | No (per-tool) | Manual |
| Auto-capture context | **Yes** | Manual editing | Manual indexing |
| Ebbinghaus decay scoring | **Yes** | No | No |
| Contradiction detection | **Yes** | No | No |
| Semantic search | **Yes** | No | Yes |
| Teams & shared context | **Yes** | No | No |
| Source tagging | **Yes** | No | Partial |
| Zero config | **Yes** | Needs maintenance | Complex setup |

---

## How It Works

### Ebbinghaus Decay

Memory relevance is scored using Ebbinghaus forgetting curves:

```
R = e^(-t/S)
```

Where `t` is time elapsed and `S` is memory strength (based on importance + access frequency). Important, frequently-used facts stay strong. Old noise fades naturally. No manual cleanup needed.

### Contradiction Detection

Store "we use Postgres" today, then "we migrated to MySQL" next month — Smara detects the contradiction, soft-deletes the old fact, and keeps the new one. Your AI never gets confused by stale context.

### Cross-Tool Context

A decision captured in Claude Code is instantly available in Cursor, Windsurf, Codex, or any connected tool. Every memory is tagged with its source — filter by tool or see everything.

---

## REST API

Three endpoints. That's it.

### Store a memory

```bash
curl -X POST https://api.smara.io/v1/memories \
  -H "Authorization: Bearer $SMARA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "user_42",
    "fact": "Prefers Python over TypeScript for backend work",
    "importance": 0.8,
    "source": "claude-code"
  }'
```

Returns `"action": "stored"`, `"duplicate"` (cosine >= 0.985), or `"replaced"` (contradiction detected).

### Search memories

```bash
curl "https://api.smara.io/v1/memories/search?user_id=user_42&q=language+preferences" \
  -H "Authorization: Bearer $SMARA_API_KEY"
```

Results ranked by semantic similarity × Ebbinghaus decay. Recent + relevant beats old + relevant.

### Get context

```bash
curl "https://api.smara.io/v1/users/user_42/context?q=preferences" \
  -H "Authorization: Bearer $SMARA_API_KEY"
```

Returns pre-formatted context ready to inject into LLM system prompts.

> Full API reference: [api.smara.io/docs](https://api.smara.io/docs)

---

## Try It Live

**[smara.io/playground](https://smara.io/playground)** — interactive demo where you can store, recall, and see contradiction detection in action. No signup required.

---

## Integrations

| Tool | Method | Status |
|------|--------|--------|
| **Claude Code** | MCP server | **Live** |
| **Cursor** | MCP server | **Live** |
| **Windsurf** | MCP server | **Live** |
| **CrewAI** | Python SDK | [smara-crewai](https://github.com/smara-io/api/tree/main/integrations/crewai) |
| **LangChain** | Python SDK | [smara-langchain](https://github.com/smara-io/api/tree/main/integrations/langchain) |
| **Paperclip** | Plugin | [paperclip-plugin](https://github.com/smara-io/paperclip-plugin) |
| **OpenAI / Codex** | REST API | Use endpoints directly |

---

## Pricing

| | **Free** | **Developer** | **Pro** |
|---|:---:|:---:|:---:|
| Memories | 10,000 | 200,000 | 2,000,000 |
| Teams | 1 team / 3 members | 3 teams / 10 members | Unlimited / 50 members |
| AI Agents | 2 | 10 + 5 custom skills | Unlimited |
| Price | **$0** | **$19/mo** | **$99/mo** |

Compare: Mem0 Pro $249/mo · Letta Max $200/mo · Zep Flex+ $475/mo

<div align="center">

**[Get your free API key](https://smara.io#signup)** — no credit card required

</div>

---

## Self-Hosting

```bash
git clone https://github.com/smara-io/api.git && cd api
VOYAGE_API_KEY=your-key docker compose up -d
```

Runs on `localhost:3011`. Requires PostgreSQL 15+ with pgvector and a [Voyage AI](https://voyageai.com) key for embeddings.

See [smara-io/api](https://github.com/smara-io/api) for full setup instructions.

---

## Packages

| Package | Description |
|---------|-------------|
| [`@smara/mcp-server`](https://npmjs.com/package/@smara/mcp-server) | MCP server for Claude Code, Cursor, Windsurf |
| [`smara-io/api`](https://github.com/smara-io/api) | Self-hostable API server (Docker + manual) |
| [`smara-io/paperclip-plugin`](https://github.com/smara-io/paperclip-plugin) | Paperclip AI plugin |

---

## Learn More

- [Why Your AI Tools Forget Everything](https://smara.io/blog/persistent-memory-for-ai-tools) — the science behind persistent AI memory
- [Mem0 vs Smara](https://smara.io/blog/mem0-vs-smara) — honest feature & pricing comparison
- [Add Memory to Claude Code](https://smara.io/blog/add-memory-to-claude-code) — 30-second setup guide
- [How Ebbinghaus Curves Make AI Smarter](https://dev.to/smara/how-ebbinghaus-forgetting-curves-make-ai-agents-smarter-ef3) — technical deep dive

---

## Feedback

Smara is in active development. We read every submission:

- [Request a feature](https://github.com/smara-io/smara/issues/new?template=feature_request.md)
- [Report a bug](https://github.com/smara-io/smara/issues/new?template=bug_report.md)
- Email: sri@smara.io
- Twitter: [@SmaraMemo](https://twitter.com/SmaraMemo)

---

<div align="center">

**smara** — *स्मर* — the act of remembrance

[Website](https://smara.io) · [API Docs](https://api.smara.io/docs) · [Playground](https://smara.io/playground) · [npm](https://npmjs.com/package/@smara/mcp-server) · [Twitter](https://twitter.com/SmaraMemo)

MIT License

</div>
