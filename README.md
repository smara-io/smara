# smara

**Persistent memory for AI tools.** Give Claude Code, Cursor, Windsurf, and Codex shared context that survives across sessions.

```bash
npx @smara/mcp-server --init
```

## The problem

Your AI tools forget everything between sessions. You repeat yourself — "we use Postgres", "deploy to Fly.io", "API is at /v2" — every single time.

## The fix

Smara stores context automatically as you work. Next session — in any tool — your AI already knows.

- **MCP-native**: works with any MCP-compatible client
- **Cross-tool**: context from Claude Code is available in Cursor
- **Ebbinghaus decay**: important facts persist, outdated noise fades naturally
- **Zero config**: install once, context loads automatically

## How it works

```
You code normally
       ↓
Smara captures decisions, patterns, context
       ↓
Next session (any tool) → context is already there
```

Memory relevance is scored using Ebbinghaus forgetting curves: `R = e^(-t/S)` where t is time elapsed and S is memory strength. Frequently-used facts stay strong. Old noise fades.

## Quick start

### MCP (Claude Code, Cursor, Windsurf)

```bash
npx @smara/mcp-server --init
```

### REST API

```bash
# Store a memory
curl -X POST https://api.smara.io/v1/memories \
  -H "Authorization: Bearer YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"user_id": "dev1", "fact": "Project uses Postgres on Supabase", "importance": 0.8}'

# Search memories
curl "https://api.smara.io/v1/memories/search?user_id=dev1&q=what+database"

# Get full context
curl "https://api.smara.io/v1/users/dev1/context"
```

## Pricing

| | Free | Developer | Pro |
|---|---|---|---|
| Memories | 10,000 | 100,000 | Unlimited |
| Projects | 1 | 10 | Unlimited |
| Price | $0 | $9/mo | $29/mo |

Get your API key at [smara.io](https://smara.io).

## Packages

| Package | Description |
|---------|-------------|
| [@smara/mcp-server](https://npmjs.com/package/@smara/mcp-server) | MCP server for Claude Code, Cursor, etc. |
| [smara-io/api](https://github.com/smara-io/api) | Self-hosted API server |

## Feedback

Smara is in beta. We read every submission:

- [Request a feature](https://github.com/smara-io/smara/issues/new?template=feature_request.md)
- [Report a bug](https://github.com/smara-io/smara/issues/new?template=bug_report.md)
- Email: sri@smara.io

## License

MIT
