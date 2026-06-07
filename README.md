# Agent OS

A persistent multi-agent framework built on Claude Code.

AI conversations are stateless — every session starts from zero. **Agent OS** fixes this by using markdown files as persistent memory, turning ephemeral conversations into a continuous, evolving workflow.

It's not one assistant — it's **an org you run as CEO.** You appoint a *lead* agent; the lead **delegates** to specialists, each working in its own focused context with its own memory, then reports decisions back to you. Work happens in "cycles," and every cycle reads prior state and writes updated state back to this repo.

## Get started

1. Open this repo in [Claude Code](https://claude.com/claude-code).
2. Run the onboarding:

   ```
   /onboard
   ```

That's it. Onboarding explains how the system works, interviews you about your mission, and scaffolds a squad — a lead plus its specialists — for you. The repo ships with a working **example squad** (a Marketing Director delegating to a Content Writer, SEO Analyst, and PPC Expert) so you can see the shape; onboarding adapts it to your domain or builds a new one and removes the example.

> Prefer to read first? The full mechanics live in `CLAUDE.md` (the bootloader) and `system/` (the protocols).

## How it works

- **You are the CEO.** You're not modeled as an agent — you're the boss. You set direction, own the budget, approve what ships, and mostly talk to one *lead* agent.
- **Agents** are role-based personas with an identity file, defined responsibilities, and their own memory, in `instance/agents/`. A **lead** (e.g. a Director) owns strategy and delegates; **specialists** do the deep work in their lane.
- **Delegation** is the core move: the lead breaks a goal into briefs and spawns each specialist in a *fresh context* via Claude Code's Agent tool. The specialist does focused work, keeps its own memory, and returns a concise summary the lead integrates. Separation of concerns — not raw horsepower — is why a squad beats one do-everything agent. See `system/delegation-protocol.md`.

```
CEO (you)
  └─ Marketing Director        ← the lead you talk to
       ├─ Content Writer        ← delegated: drafts
       ├─ SEO Analyst           ← delegated: research & briefs
       └─ PPC Expert            ← delegated: paid campaigns
```

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
