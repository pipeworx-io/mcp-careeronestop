# mcp-careeronestop

CareerOneStop MCP — wraps the CareerOneStop Web API (U.S. Dept. of Labor)

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1576+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `careeronestop_salary` | What does <occupation> pay in <location> — wage percentiles (10th/25th/median/75th/90th, hourly + annual) for a U.S. occupation from the Dept. of Labor CareerOneStop API. Accepts an O*NET/SOC title or code. Example: careeronestop_salary({ occupation: "Registered Nurses", location: "TX", _apiKey: "userId:token" }) |
| `careeronestop_occupation` | Occupation detail (tasks, outlook, typical education) for <occupation> from the Dept. of Labor CareerOneStop / O*NET data. Example: careeronestop_occupation({ occupation: "Registered Nurses", _apiKey: "userId:token" }) |
| `careeronestop_compare_salary` | Compare salaries across occupations — side-by-side wage data for multiple U.S. occupations from the Dept. of Labor CareerOneStop API. Example: careeronestop_compare_salary({ occupations: "Registered Nurses, Physician Assistants", location: "US", _apiKey: "userId:token" }) |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "careeronestop": {
      "url": "https://gateway.pipeworx.io/careeronestop/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/careeronestop/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1576+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "careeronestop": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-careeronestop"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-careeronestop
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Careeronestop data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
