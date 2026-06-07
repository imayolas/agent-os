# Artifact Rules

Artifacts are persistent markdown files that constitute the agent's memory. They come in two categories: **standing artifacts** (always exist, reflect current reality) and **project files** (born from work, deleted when done).

Both `system/` and `instance/` are git-tracked, so git history is the archive. Nothing is truly lost. Be aggressive about deleting finished work.

## Standing Artifacts

These files always exist per agent. They expand and contract with reality.

### State of Mind (`instance/agents/[agent-name]/state-of-mind.md`)

- A snapshot of now: current focus, open questions, blockers, what's next.
- Rewritten (not appended) every cycle. Under 50 lines.
- The org-level equivalent lives at `instance/artifacts/state-of-mind.md` — also rewritten every cycle.

### Strategy (`instance/agents/[agent-name]/strategy.md`)

- Mid-to-long-term goals, positioning, direction.
- Created during strategy sessions (user-initiated).
- **Evolves with strategy, not with cycles.** Update when direction genuinely changes — not as a side effect of execution work. If work drifts from strategy, surface it rather than silently adjusting. Small refinements during active strategy discussion are fine; routine cycle updates are not.

### Recurring Tasks (`instance/agents/[agent-name]/recurring-tasks.md`)

- Defines repeating workflows: what, cadence, how to execute, where to log results.
- Updated as processes evolve. Not all agents need this file.

## Intelligence Files

Intelligence files live in `instance/agents/[agent-name]/intelligence/` and capture **what we know to be true about how the world works** — external principles, market insights, competitive knowledge, and domain expertise that inform decisions across projects and cycles.

Intelligence is distinct from strategy (what we'll do) and projects (work in progress). Intelligence says: "this is how things work; factor it in."

### Characteristics

- **Living documents.** Updated during and at the end of cycles as new information arrives.
- **Curated, not appended.** When new intelligence contradicts old intelligence, the old entry is deleted or rewritten — not kept alongside. Stale intelligence is worse than no intelligence because it leads to wrong decisions.
- **Actionable.** Every intelligence entry should change how the agent thinks or acts. If it doesn't influence decisions, it doesn't belong here.
- **Sourced.** External intelligence includes the source URL/reference. Internal intelligence (from our own data) cites the data source and date.

### Lifecycle

- **Create** when a durable insight emerges — from external research, from our own data analysis, or from user direction.
- **Update** every cycle where relevant work happens. Ask: "Did I learn anything this cycle that changes what I know?"
- **Delete** when information becomes outdated, contradicted, or irrelevant. Keeping dead intelligence around is how agents fall into the generic trap — acting on stale assumptions instead of current reality.
- **Never hoard.** An intelligence file that hasn't been updated or referenced in 3+ months should be reviewed for relevance.

### Naming

Files are named by topic: `ai-seo.md`, `seo-content-principles.md`, `competitive-landscape.md`. No date prefix — intelligence is timeless until it isn't, at which point it gets updated or deleted.

### Structure

Every intelligence file starts with frontmatter:

```
---
title: [Topic]
type: intelligence
updated: YYYY-MM-DD
last_challenged: YYYY-MM-DD
description: [One line — what decisions does this intelligence inform?]
---
```

The `last_challenged` field tracks when the entry was last actively red-teamed (not just updated). Added after the first challenge. If `last_challenged` is more than 5 cycles old during a strategy session, challenge is overdue.

After frontmatter, structure is free-form but should be scannable. Use headers for distinct insights. Include source references.

### How to Use Intelligence (Critical)

Intelligence informs decisions — it does not make them. This distinction is the difference between a system that learns and one that calcifies. See `CLAUDE.md` → Cognitive Discipline for the full framework. Key rules:

1. **Intelligence is a hypothesis, not a fact.** Every entry is the best understanding at the time it was written. It may be wrong, incomplete, or outdated by the time it's read. Treat it as input to reasoning, never as a substitute for reasoning.

2. **Data-first ordering.** During analysis, reason from raw data before consulting intelligence. Form preliminary conclusions independently, then cross-reference. This prevents anchoring — the cognitive bias where early inputs disproportionately shape all subsequent reasoning.

3. **Contradiction surfacing.** When cycle work reveals evidence that conflicts with an intelligence entry, the agent is forbidden from reconciling it silently. Both the intelligence claim and the contradicting evidence must be stated explicitly to the user. The user decides what to update.

4. **Challenge cadence.** Intelligence files include a `last_challenged` date in their frontmatter (added after first challenge). During strategy sessions and at least once every 5 cycles, agents must actively argue against each intelligence entry using current data. This is red-teaming, not review — the goal is to find reasons the intelligence might be wrong. Entries that survive challenges are stronger. Entries that don't are updated or deleted.

5. **No confirmation loops.** If an agent's reasoning follows the pattern "the intelligence says X, therefore X" — that is anchoring, not analysis. Intelligence provides context and principles; conclusions must be justified by current evidence.

---

## Project Files

Project files live in `instance/agents/[agent-name]/projects/` and track active initiatives — a website rewrite, a campaign launch, a research effort. They are working documents.

### Naming

Files are named: `yyyy-mm-<descriptive-name>.md`

The date is the month the project started. The name should be verbose enough that `ls` tells you what's going on at a glance.

Examples:

- `2026-03-website-rewrite-joyfe-landing-pages.md`
- `2026-02-seo-technical-audit-and-fixes.md`
- `2026-03-competitive-backlink-analysis.md`

### Structure

Every project file starts with a frontmatter header. This enables fast AI discovery — an agent can `read --limit 15` to decide if a file is relevant without loading the full document. It is also the single source of truth for cross-project aggregation (see `bin/status`).

**Required fields:**

- `title` — human-readable name
- `status` — `active` | `paused` | `done`
- `owner` — agent name (matches the parent directory under `instance/agents/`)
- `started` — YYYY-MM-DD (month the project started)
- `updated` — YYYY-MM-DD (last meaningful edit)
- `description` — one-line purpose

**Optional fields:**

- `blocked_on` — short phrase describing what is blocking progress, or omit when not blocked

Projects may add other fields as needed (e.g., `data_source`), but the required set above must always be present.

```
---
title: Website Rewrite — Joyfe Landing Pages
status: active
owner: marketing-seo-specialist
started: 2026-03-04
updated: 2026-03-04
description: Redesign and rewrite all Joyfe school landing pages for better conversion and SEO performance.
blocked_on: Awaiting brand-color confirmation from CEO
---
```

After the frontmatter, structure is free-form. Use whatever fits the project: task checklists, notes, decisions, references. Keep it practical.

### Lifecycle

- **Create** when a multi-cycle initiative starts.
- **Update** freely during execution.
- **Delete** when the project is done. Git remembers. Don't hoard finished work.
- Set `status: done` before deleting if you want a clean final commit message.

## General Rules

1. **Delete finished work.** The archive is git history. No "\_archive" sections, no zombie files.
2. **Keep files self-contained.** Each file should make sense on its own.
3. **Strategy evolves with strategy.** Treat strategy docs as authoritative direction. Update them when strategy genuinely evolves, not on every cycle.
4. **No index files.** Discovery happens via `ls` and `glob`. Verbose file names and frontmatter replace manually maintained indexes.
