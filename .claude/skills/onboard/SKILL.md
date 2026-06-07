---
name: onboard
description: Onboard a new user to this Agent OS repo. Explains what the framework is, how squad-based delegation works, how to operate it day-to-day, then interviews the user (who is the CEO) and scaffolds their own agent squad. Use when the user runs /onboard, says they're new here, asks "what is this repo / how does this work / how do I get started", or has just cloned the repo.
---

# Onboarding

Your job: get a brand-new user from "I just cloned this repo" to "I have my own agent squad and I know how to run it." Be warm, concrete, and brief. This is a conversation, not a lecture — explain, then **ask**, then **build**.

The single most important idea to land: **this is not one assistant — it's an org you run.** The user is the CEO. They appoint a lead agent, who delegates to specialists. If the user walks away understanding only that, onboarding succeeded.

## Step 1 — Orient (read before you speak)

Quietly read these so your explanation reflects the *actual* current state, not assumptions:

- `CLAUDE.md` — the bootloader.
- `system/cycle-protocol.md`, `system/artifact-rules.md`, `system/delegation-protocol.md` — the core mechanics.
- `ls instance/agents/` and skim `instance/agents/marketing-director/marketing-director.md` — the bundled **example marketing squad** (Marketing Director → Content Writer, SEO Analyst, PPC Expert). Also skim `instance/agents/marketing-director/projects/2026-06-example-campaign.md` — it shows the delegation loop in action.

Don't dump these at the user. Use them so what you say is true right now.

## Step 2 — Explain (keep it tight, then stop)

Tell the user, in your own words:

- **What this is.** Agent OS is a persistent, multi-agent framework on Claude Code. Normally every Claude conversation starts from zero; here, memory lives in markdown files in this repo, so work continues across sessions.
- **You are the CEO.** You don't model yourself as an agent — you *are* the boss. You set direction, own the budget, and approve what ships. You mostly talk to one **lead agent**.
- **The lead delegates to a squad.** The lead (e.g. a Director) doesn't do specialist work itself — it breaks a goal into briefs and **delegates** to specialists, each of whom runs in its own fresh context, does deep work, keeps its own memory, and returns a concise summary. The lead integrates the pieces and brings *decisions* back to you. This separation — not raw horsepower — is what makes a squad beat one do-everything agent.
- **The building blocks.** *Agents* (role-based personas with their own memory, in `instance/agents/`), *Cycles* (one working session = one conversation; start with *"New cycle for [Agent]"*), *Artifacts* (the markdown memory: `state-of-mind.md`, `strategy.md`, `projects/`, `intelligence/`), *Skills* (knowledge packs loaded on demand).
- **system/ vs instance/.** `system/` is the portable engine (don't customize lightly). `instance/` is *your* deployment.

Point at the example: "There's a working example squad in here — a Marketing Director with three specialists — purely to show the shape. We'll either adapt it to your world or build a different squad and delete it."

## Step 3 — Interview (design their squad)

You're designing a **team**, not a single agent. Lead with one focused question:

> **What's the mission — and who's on the team to deliver it?** Tell me what you want this venture to achieve, and I'll help you shape a lead + the specialists under them. (Don't overthink it — we'll refine together.)

Then draw out, conversationally (one or two questions at a time, never a wall):

1. **The mission / domain.** What is the CEO trying to get done? This defines the squad's purpose.
2. **The lead.** What's the lead role called, and what does it own? (Director of X, Head of Y, Chief of Staff…)
3. **The specialists.** What 2–4 distinct jobs does the lead delegate? Each should be a genuinely different skill (so a fresh-context specialist adds value), not a slice of the same job. Name them.
4. **The handoffs.** How does work flow between them? (e.g. researcher briefs → writer drafts → lead reviews.) This is the teamwork story — capture it.
5. **Sources of truth.** Any codebase, dataset, or external service (via MCP) the squad consults? If MCP: point them at `instance/mcp-setup.md` and `.mcp.json.example`.
6. **Rules & vocabulary.** Anything for `instance/rules.md` or `instance/glossary.md`.

**Start small is fine.** If the user is unsure, ship the lead + one specialist now and tell them adding more later is a 2-file operation. Don't force a big org up front.

**If their domain *is* marketing**, offer to adapt the existing example squad in place rather than rebuild from scratch.

## Step 4 — Scaffold the squad

Build it by mirroring the bundled example (read those files as templates — the lead and the specialists have deliberately different shapes):

- **The lead** → `instance/agents/<lead>/<lead>.md`. Mirror `marketing-director.md`: a "Who you report to" section naming **the CEO (the user)**, Role, Personality, Responsibilities, a **Delegation table** listing each specialist + when to delegate to them, Files Owned, and an Initialization Checklist. Give it a `state-of-mind.md`, and a short `strategy.md` if strategy matters.
- **Each specialist** → `instance/agents/<specialist>/<specialist>.md`. Mirror `content-writer.md` / `seo-analyst.md`: a "reports to [the lead]" line, narrower Role, a "What You Do NOT Do" section, Files Owned, Initialization Checklist. Give each a `state-of-mind.md`.
- Create `projects/` (and `intelligence/`/`skills/` where useful; `.gitkeep` any you leave empty).
- Use lowercase-hyphenated folder names.

Then update shared state:

- `instance/artifacts/state-of-mind.md` — replace the example org snapshot with the real one: the CEO, the lead, and who reports to whom.
- `instance/rules.md` / `instance/glossary.md` — fill in any domain rules/terms; leave the rest as placeholders.

## Step 5 — Retire the example

If you built a *new* squad (didn't adapt the marketing one), offer to remove the example:

> "Want me to delete the example marketing squad now that yours exists? (`rm -rf instance/agents/marketing-director instance/agents/content-writer instance/agents/seo-analyst instance/agents/ppc-expert`)"

Only delete on a clear yes.

## Step 6 — Hand off

Close by showing them how to run their org:

- **Start a cycle:** *"New cycle for [lead agent]"*. The lead loads its context and either follows your instructions or proposes what the squad should do.
- **Delegation just happens:** when the lead hits specialist work, it spawns that specialist for you — you don't manage it by hand.
- **Status across the squad:** run `bin/status`.
- **Strategy work:** *"Strategy session for [lead]"*.
- **Monthly upkeep (don't skip this):** roughly once a month, run `/dream-instance` (and, less often, `/dream-system`). These "dream" passes consolidate accumulated memory and delete what's gone stale — they keep the system from rotting into contradictory, ignored cruft. `CLAUDE.md` nudges you at cycle start when one is overdue.

Remind them: their org now persists in this repo — commit it to git so it moves between machines and is recoverable.

## Tone guardrails

- Don't overwhelm. Explain in small pieces, check understanding, then proceed.
- Keep reinforcing the mental model: **CEO → lead → specialists.**
- Don't invent business specifics — ask.
- Prefer doing over instructing: you create the files, you don't tell them to.
