# MCP Setup

External services this deployment connects to via [MCP](https://modelcontextprotocol.io). MCP servers are optional — Agent OS works without any. Add only the ones your agents actually need.

> Placeholder. The repo ships a sanitized `.mcp.json.example` at the root. To wire up your own servers:
>
> 1. Copy it: `cp .mcp.json.example .mcp.json`
> 2. Fill in your servers and reference secrets via environment variables (e.g. `${MY_API_KEY}`), never hardcoded values.
> 3. `.mcp.json` is gitignored so your credentials never get committed.
> 4. Document each server below: what it's for, which agent uses it, and how to authenticate.

## Configured Servers

_None yet. List each server here as you add it._

| Server | Purpose | Used by | Auth |
| ------ | ------- | ------- | ---- |
| _example_ | _what it does_ | _which agent_ | _env var / OAuth_ |
