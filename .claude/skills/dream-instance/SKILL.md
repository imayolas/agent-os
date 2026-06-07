---
name: dream-instance
description: |
  Monthly consolidation pass over `instance/`. Reviews cycles, agents, projects, intelligence, and state-of-mind across the last month; cross-references claims against the actual codebase, docs, and git history; surfaces patterns; and aggressively compresses or deletes stale content with the user's confirmation. Inspired by Anthropic's Dreaming for Claude Managed Agents, but biased toward forgetting. Use when the user says "dream", "consolidate the instance", or "/dream-instance".
user-invocable: true
---

# Dream — Instance

Instance memory accumulates. Closed projects linger. Intelligence files outlive the assumptions that made them true. State-of-mind items go stale. This skill is a scheduled forgetting pass: it reads everything under `instance/`, compares it to reality, and shrinks it.

Bias is toward deletion. Git is the archive. If a dream is too aggressive, the next cycle won't suffer — committed history is recoverable. Erring toward forgetting is acceptable.

**Run this on a recurring monthly cadence — it is not a one-off.** Skipping it lets stale projects, outdated intelligence, and dead state-of-mind items pile up until they actively mislead. `CLAUDE.md`'s Dream Cadence Check nudges you when it's overdue.

## When to run

- The user invokes `/dream-instance`, says "dream", or asks to consolidate the instance.
- Default cadence: monthly. Read `instance/dreams/log.md` and find the latest entry with `scope: instance`. If <30 days old, ask the user before force-running. If the log file is missing, create it and proceed as the first dream.

## Scope

Touches only `instance/`. For `system/`, use `dream-system`.

## Procedure

### 1. Cadence check

Read `instance/dreams/log.md`. Locate the most recent `## Dream YYYY-MM-DD — instance` entry.

- Missing file → create `instance/dreams/log.md` with a one-line header. First dream.
- <30 days old → ask: "Last instance dream was N days ago. Force-run anyway?"
- Otherwise → proceed.

### 2. Gather inputs (data-first)

Read raw evidence before opinions. Follow `CLAUDE.md` → Cognitive Discipline → Data-first ordering.

1. `instance/cycles/log.md` (full)
2. `instance/artifacts/state-of-mind.md`
3. Every `instance/agents/*/state-of-mind.md`
4. Every `instance/agents/*/projects/*.md` — frontmatter first; load full file only if it becomes a candidate
5. Every `instance/agents/*/intelligence/*.md` — frontmatter first
6. Every `instance/agents/*/recurring-tasks.md`
7. `instance/rules.md`, `instance/glossary.md`

### 3. Reality check (mandatory)

For every claim made in a project, intelligence file, or state-of-mind item, find evidence in the real world:

- **Code claims** ("built X", "implemented Y") → `git log --oneline --grep=...` and `git log -- <path>`; check the file exists in whatever codebase this deployment works on (see `instance/rules.md` for where that is).
- **Output claims** ("doc written", "page launched", "report delivered") → check wherever the deliverable is supposed to live (a repo, a site, a file store) and any URLs the project links to.
- **External claims** ("got data from Y", "talked to Z") → check timestamps; if unverifiable, mark for user confirmation.

Write down every divergence before doing anything else. Reality-vs-state mismatch is the single most valuable signal of the dream. Per `CLAUDE.md` → Cognitive Discipline → "Surface contradictions, never reconcile silently", state both sides explicitly — never silently reconcile.

### 4. Pattern extraction

Across all inputs, look for three patterns:

- **Recurring mistakes** — the same blocker, retry, or correction in 3+ cycles. Candidate for a new rule in `instance/rules.md` or a new skill.
- **Converged workflows** — the same kind of task tackled the same way across 3+ cycles. Candidate for promotion into `recurring-tasks.md` or a skill.
- **Preference drift** — the user repeatedly corrected the same kind of decision. Candidate for `instance/rules.md`.

### 5. Generate forgetting candidates

Default candidates for deletion or compression:

- Projects with `status: done` → delete.
- Projects with `status: active` but no commits, files, or content match the deliverable → ask user.
- Projects with `updated:` older than 90 days → ask user.
- Intelligence files not updated in 90+ days **and** `last_challenged` more than 5 cycles back → ask user.
- Intelligence files never referenced in `instance/cycles/log.md` → ask user.
- State-of-mind items unchanged across the last 3 cycles → propose removal.
- Recurring tasks with no log evidence of ever firing → propose removal.
- `instance/glossary.md` entries not used anywhere in 6+ months → propose removal.

### 5b. Done projects must be deleted, not archived

**Rule (user directive, 2026-05-27):** A project that is done must be deleted from `projects/`. Do not keep it with `status: done` "for reference" — that grows the surface area indefinitely and rots over time. Git keeps every prior version.

**Before deleting, extract durable learnings to their permanent home:**

| Type of learning                              | Permanent home                                                             |
| --------------------------------------------- | -------------------------------------------------------------------------- |
| Methodology, framework, reusable how-to       | `instance/agents/<agent>/intelligence/` or `instance/artifacts/` |
| Market / customer / competitor facts          | `intelligence/` of the relevant agent                                      |
| Business rule, preference, constraint         | `instance/rules.md` (or `system/agent-rules.md` if generic)       |
| Vocabulary                                    | `instance/glossary.md` (or `system/glossary.md` if generic)       |
| Current status / next-step context            | `state-of-mind.md` (agent or org)                                          |
| Historical decisions and outcome              | The cycle log entry (already there — don't re-record)                      |

If nothing in a "done" project is durable, just delete it. Do not promote shallow snapshots ("a baseline pulled on date X") into intelligence — those go to the bin.

Methodology / reference docs that have no end-state (e.g. evaluation frameworks, content-style guides) do **not** belong in `projects/` long-term — they should be moved to `intelligence/` or `artifacts/` instead. If you find one parked in `projects/` with no plausible "done" condition, move it out as part of the dream.

### 6. Interactive Q&A loop

For each candidate, ask the user with full context. Group related candidates into batches of 3–5 — do not fire 30 questions at once.

Use these question shapes:

- **Has this been implemented?** — for project items where code or content should exist but doesn't.
  > "Project `2026-02-spanish-localization` is `status: active`. I scanned the target repo and site — nothing shipped. Implemented elsewhere, dismissed, or delete?"
- **Has this been done?** — for tasks or checklists with unclear status.
  > "State-of-mind says 'awaiting confirmation from CEO on brand colors.' That entry has been there since cycle 8 (six cycles ago). Done, still pending, or remove?"
- **Dismiss?** — for things the user may have moved past.
  > "Intelligence file `competitive-backlinks.md` was last updated in February and hasn't been referenced in the cycle log since. Still load-bearing, compress, or dismiss?"
- **Delete?** — for clear closures.
  > "Project `2026-01-twitter-launch.md` has `status: done`. Delete? (git keeps it.)"

The user's answer is authoritative. If they say "dismiss", delete it. If they say "keep", bump the `updated:` field and move on. Never lobby for a deletion the user declined.

### 7. Apply approved changes

After each batch of Q&A, apply the approved deletions and compressions immediately. Prefer a recoverable delete (a trash tool like the `trash` CLI if you have one; otherwise `rm` is fine — git keeps every prior version). Update remaining files' `updated:` frontmatter where touched.

For compressions, propose the diff before applying — show the user what survives.

### 8. Write the dream log entry

Prepend a new entry to `instance/dreams/log.md`:

```
## Dream YYYY-MM-DD — instance
**Scope:** instance
**Cycles reviewed:** N (cycles X–Y)
**Reality-check divergences:** [count, brief notes]
**Patterns surfaced:**
- [recurring mistake / converged workflow / preference drift]
**Deleted:** [paths]
**Compressed:** [paths]
**Added:** [paths]
**User dismissed:** [count] candidates
```

### 9. Surface follow-ups

If a pattern is too big to act on inside the dream — "this looks like it needs a new agent", "this is a strategy question", "this conflicts with `system/`" — surface it to the user as a final note. Do not silently act on it.

## Constraints

- **Deletion is encouraged. Confirmation is mandatory.** Aggressive in proposals, conservative in actions.
- **Prefer a recoverable delete.** Use a trash tool (e.g. the `trash` CLI) if available; otherwise `rm` is fine — committed git history is the real safety net if a dream went too far.
- **Touch only `instance/`.** Engine changes route to `dream-system`.
- **Never reconcile contradictions silently.** When intelligence and reality disagree, state both. The user decides.
- **Do not duplicate per-cycle meta-improve.** That step (`cycle-protocol.md` § 5) operates inside one cycle. Dreaming operates across many.

## Output

A leaner `instance/`, a dream log entry, and a short verbal summary: files reviewed, files deleted, biggest pattern, anything that needs follow-up.
