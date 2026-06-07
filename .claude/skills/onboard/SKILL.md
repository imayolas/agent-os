---
name: onboard
description: Onboard a new user to this Agent OS repo. Explains what the framework is, how it works, how to operate it day-to-day, then interviews the user and scaffolds their first real agent. Use when the user runs /onboard, says they're new here, asks "what is this repo / how does this work / how do I get started", or has just cloned the repo.
---

# Onboarding

Your job: get a brand-new user from "I just cloned this repo" to "I have my own first agent and I know how to run a cycle." Be warm, concrete, and brief. This is a conversation, not a lecture — explain, then **ask**, then **build**.

## Step 1 — Orient (read before you speak)

Quietly read these so your explanation reflects the *actual* current state, not assumptions:

- `CLAUDE.md` — the bootloader.
- `system/cycle-protocol.md` and `system/artifact-rules.md` — the core mechanics.
- `ls instance/agents/` — what agents currently exist (a fresh clone has only `example-researcher`).

Don't dump these at the user. Use them to make sure what you say is true right now.

## Step 2 — Explain (keep it to ~150 words, then stop)

Tell the user, in your own words:

- **What this is.** Agent OS is a persistent multi-agent framework built on Claude Code. Normally every Claude conversation starts from zero. Here, memory lives in markdown files in this repo, so work continues across sessions.
- **The four building blocks.**
  - **Agents** — role-based personas (e.g. a Researcher, a Marketer) with an identity file, their own memory, and owned artifacts. They live in `instance/agents/`.
  - **Cycles** — one working session = one Claude conversation. A cycle reads prior state, does work, and writes updated state. You start one by saying *"New cycle for [Agent Name]"*.
  - **Artifacts** — the markdown memory: `state-of-mind.md` (current focus), `strategy.md`, `projects/` (active work), and `intelligence/` (durable knowledge).
  - **Skills** — knowledge packs loaded on demand.
- **The split that matters.** `system/` is the portable engine (don't customize lightly). `instance/` is *your* deployment — your agents, your rules, your work.

Then say: there's a sample agent (`example-researcher`) here purely to show the structure, and you'll help them replace it with a real one now.

## Step 3 — Interview (the introductory question)

Ask the user **one focused opening question**, then follow up as needed. Lead with this:

> **What do you want your first agent to *do* for you?** Describe the job in a sentence or two — e.g. "keep my competitive research up to date," "draft and manage my newsletter," "triage and summarize my reading list." Don't worry about getting it perfect; we'll shape it together.

From their answer, draw out just enough to define an agent:

1. **Role & name** — what should it be called? (e.g. "Research Lead", "Content Writer")
2. **Responsibilities** — the 3–5 things it owns.
3. **Tone/personality** — how should it think and write?
4. **Sources of truth** — is there a codebase, dataset, or external service (via MCP) it should consult? (If MCP: point them at `instance/mcp-setup.md` and `.mcp.json.example`.)
5. **Domain vocabulary or rules** — anything that belongs in `instance/rules.md` or `instance/glossary.md`.

Keep it conversational. Ask follow-ups one or two at a time, not as a wall of questions.

## Step 4 — Scaffold their first agent

Once you understand the role, create it by mirroring the structure of `example-researcher` (read it as a template):

- `instance/agents/<agent-name>/<agent-name>.md` — identity file with Role, Personality, Responsibilities, Files Owned, and an Initialization Checklist. Use a lowercase-hyphenated folder name.
- `instance/agents/<agent-name>/state-of-mind.md` — seed it with their stated goal as the current focus.
- Create `projects/`, `intelligence/`, and `skills/` subfolders (a `.gitkeep` in any you leave empty).

Then update the shared state to reflect reality:

- `instance/artifacts/state-of-mind.md` — replace the placeholder org snapshot with the real agent and priority.
- `instance/rules.md` and `instance/glossary.md` — fill in any domain rules/terms they gave you (leave the rest as placeholders).

## Step 5 — Offer to retire the example

Ask whether to delete the sample agent now that a real one exists:

> "Want me to remove the `example-researcher` sample agent? It was only there to show the structure. (`rm -rf instance/agents/example-researcher`)"

Only delete on a clear yes.

## Step 6 — Hand off

Close by telling them exactly how to start working:

- **Start a cycle:** say *"New cycle for [their agent name]"*. The agent will load its context and either follow instructions or propose what to do.
- **Check status across projects:** run `bin/status`.
- **Strategy work:** *"Strategy session for [agent]"*.
- **Monthly upkeep (don't skip this):** roughly once a month, run `/dream-instance` (and, less often, `/dream-system`). These "dream" passes consolidate accumulated memory and delete what's gone stale — they're what keep the system from slowly rotting into contradictory, ignored cruft. `CLAUDE.md` will nudge you at the start of a cycle when one is overdue. It matters more the longer you run the system.

Remind them their work now persists in this repo — commit it to git so it moves between machines and is recoverable.

## Tone guardrails

- Don't overwhelm. Explain in small pieces, check understanding, then proceed.
- Don't invent business specifics — ask.
- Prefer doing over instructing: you create the files, you don't tell them to.
