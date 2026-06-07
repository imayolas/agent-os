# Agent OS

A persistent multi-agent framework built on Claude Code.

AI conversations are stateless — every session starts from zero. **Agent OS** fixes this by using markdown files as persistent memory, turning ephemeral conversations into a continuous, evolving workflow. You define role-based agents, work in "cycles," and every cycle reads prior state and writes updated state back to this repo.

## Get started

1. Open this repo in [Claude Code](https://claude.com/claude-code).
2. Run the onboarding:

   ```
   /onboard
   ```

That's it. Onboarding explains how the system works, interviews you about what you want your first agent to do, and scaffolds it for you. It will also offer to remove the bundled `example-researcher` sample agent once you have a real one.

> Prefer to read first? The full mechanics live in `CLAUDE.md` (the bootloader) and `system/` (the protocols).

## How it works

- **Agents** are role-based personas (a Researcher, a Marketer, …) with an identity file, defined responsibilities, and their own memory. They live in `instance/agents/`.
- **Cycles** are individual Claude Code conversations. Start one by saying *"New cycle for [Agent Name]"*. Each cycle reads prior state, does work, and writes updated state.
- **Artifacts** are persistent markdown files — `state-of-mind.md` (current focus), `strategy.md`, `projects/` (active work), and `intelligence/` (durable knowledge) — that carry context across cycles.
- **Skills** are knowledge packs loaded on demand (in `.claude/skills/`).
- **Dreams** are monthly consolidation passes (`/dream-instance`, `/dream-system`) that compress accumulated memory, biased toward forgetting.

## The system / instance split

- **`system/`** is the portable engine — protocols, rules, glossary. Project-agnostic; change it deliberately (and log changes in `system/changelog.md`).
- **`instance/`** is *your* deployment — your agents, your rules, your work. This is where almost everything you do lives.

## Repo structure

```
├── CLAUDE.md              # Bootloader (read every session)
├── system/                # Portable framework: protocols, rules, glossary, changelog
├── instance/              # Your deployment (the "soul")
│   ├── agents/            #   role-based agents + their memory
│   ├── artifacts/         #   org-level shared state
│   ├── cycles/            #   append-only cycle log + coordination state
│   ├── dreams/            #   monthly consolidation log
│   ├── rules.md           #   your business/domain rules
│   └── glossary.md        #   your domain vocabulary
├── bin/status             # Cross-project status board (reads project frontmatter)
├── .claude/skills/        # onboard, dream-instance, dream-system
└── .mcp.json.example      # Template for optional MCP servers (copy to .mcp.json)
```

## Storage model

Everything is git-tracked. `system/` evolves slowly; `instance/` changes every cycle. Commits are how state moves between machines and how anything deleted during a "dream" stays recoverable. The repo is standalone — clone it, run `/onboard`, and go.

## Optional: MCP servers

Agents can use external tools via [MCP](https://modelcontextprotocol.io) (search, analytics, Notion, etc.). It's entirely optional. To wire up your own: `cp .mcp.json.example .mcp.json`, fill in your servers (secrets via `${ENV_VAR}`), and document them in `instance/mcp-setup.md`. The real `.mcp.json` is gitignored.

## License

MIT — see [LICENSE](LICENSE).
