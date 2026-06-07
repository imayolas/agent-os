---
name: dream-system
description: |
  Monthly consolidation pass over `system/` — the engine. Reviews protocols, rules, glossary, and changelog against how they actually show up in cycle log evidence; surfaces worked-around rules, missing rules, dead content, and boundary violations; and proposes deletions, compressions, and revisions. All changes are proposed-only — never applied silently — per the threshold rule in `cycle-protocol.md` § 5c. Use when the user says "dream the system", "consolidate the engine", or "/dream-system".
user-invocable: true
---

# Dream — System

The engine accumulates rules. Some get worked around, some never fire, some are superseded but never deleted. This skill is a scheduled forgetting pass over `system/`: it reads every protocol and rule, checks how each one shows up in actual cycle behavior, and proposes deletions, compressions, and revisions.

Bias is toward forgetting. Git is the archive. The engine is real only insofar as cycles use it — rules that don't show up in cycle evidence are candidates for removal.

**Run this on a recurring cadence — monthly, or whenever the engine has drifted from how cycles actually behave.** It is not a one-off. The system nudges you (via `CLAUDE.md`'s Dream Cadence Check) when a system dream is overdue; the engine changes slowly, so this can run less often than `dream-instance`.

**Threshold rule:** all changes to `system/` are proposed to the user with a diff, applied only after explicit yes. No bulk-apply, no silent edits. This matches `cycle-protocol.md` § 5c.

## When to run

- The user invokes `/dream-system`, says "dream the system", or asks to consolidate the engine.
- Default cadence: monthly. Read `instance/dreams/log.md` and find the latest entry with `scope: system`. If <30 days old, ask the user before force-running. If the log file is missing, create it and proceed as the first dream.

## Scope

Touches only `system/` (and writes one entry to `instance/dreams/log.md` plus `system/changelog.md`). For instance-level cleanup, use `dream-instance`.

## Procedure

### 1. Cadence check

Read `instance/dreams/log.md`. Locate the most recent `## Dream YYYY-MM-DD — system` entry.

- Missing file → create it, first dream.
- <30 days old → ask: "Last system dream was N days ago. Force-run anyway?"
- Otherwise → proceed.

### 2. Gather inputs (data-first)

1. Every `system/*.md` — `agent-rules.md`, `artifact-rules.md`, `cycle-protocol.md`, `delegation-protocol.md`, `glossary.md`, `changelog.md`.
2. `system/changelog.md` — history of engine changes; what was tried, what stuck, what didn't.
3. `instance/cycles/log.md` — the engine is judged by how it shows up in actual cycles.
4. `.claude/skills/` listing — confirm skills referenced in system protocols still exist.
5. `CLAUDE.md` and the project-root `CLAUDE.md` for cross-references.

### 3. Reality check (mandatory)

The engine is real only insofar as cycles use it. For every rule, protocol step, or skill referenced in `system/*.md`:

- **Is it being followed?** Grep `cycles/log.md` for evidence of the rule firing.
- **Has it been worked around?** Search cycle entries for friction, edits, or notes like "this didn't apply", "I deviated from X".
- **Is it dead?** A rule that hasn't fired or been referenced in 10+ cycles is a candidate for removal.

Cross-reference `system/changelog.md`: for each recent engine change, did it achieve its stated intent? If not, it's a roll-back candidate.

Write down every divergence before doing anything else. Per `CLAUDE.md` → Cognitive Discipline → "Surface contradictions, never reconcile silently."

### 4. Pattern extraction

- **Worked-around rules** — keeps appearing in cycle summaries as friction → revise or remove.
- **Missing rules** — recurring cycle behavior that should be codified but isn't → propose addition.
- **Boundary violations** — generic content in `instance/`, project-specific content in `system/` → propose moves.
- **Dead glossary terms / dead skill references** → propose removal.
- **Redundant or superseded steps** — the same outcome is reached two different ways → propose unification.

### 5. Generate forgetting candidates

Default candidates:

- Sections of `system/*.md` that haven't fired or been referenced in 10+ cycles.
- `system/changelog.md` entries older than 6 months → propose compression to a year-summary.
- `system/glossary.md` entries not used in any cycle log entry → propose removal.
- Skills referenced in protocols but missing from `.claude/skills/` → either restore the skill or remove the reference.
- Protocol steps that have been superseded by newer skills — if a section's job is now done by a skill, the section is a removal candidate.

### 6. Interactive Q&A loop

For each candidate, ask the user with full context. Batch into 3–5 questions at a time.

Question shapes:

- **Still load-bearing?** — for rules that haven't fired in 10+ cycles.
  > "`agent-rules.md` § 'Delegated cycle pauses' — no cycle log entry references this rule. Still wanted, or remove?"
- **Worked around — revise or remove?** — for friction patterns.
  > "Cycles 12, 14, 17 all flagged rule X as confusing. Reword as Y, replace with Z, or remove?"
- **Compress?** — for verbose system files that have stabilized.
  > "`changelog.md` is 200 lines, the bottom 80% is older than 3 months. Compress to a year-summary?"
- **Move?** — for boundary violations.
  > "`instance/rules.md` contains a rule that looks generic ('never use em dash in body copy'). Move to `system/agent-rules.md` so other deployments inherit it?"
- **Has this been done?** / **Dismiss?** — same shapes as `dream-instance`, applied to engine items.

### 7. Apply approved changes — proposed-only

Every `system/` change is shown as a diff before applying. Only apply after explicit user yes. No bulk-apply.

For each applied change, write a one-line entry to `system/changelog.md` (per `cycle-protocol.md` § 5c).

### 8. Write the dream log entry

Prepend a new entry to `instance/dreams/log.md`:

```
## Dream YYYY-MM-DD — system
**Scope:** system
**Files reviewed:** [list]
**Cycles cross-referenced:** N (cycles X–Y)
**Reality-check divergences:** [count, brief notes]
**Patterns surfaced:**
- [worked-around rule / missing rule / dead content / boundary violation]
**Deleted:** [paths and sections]
**Compressed:** [paths]
**Revised:** [paths]
**User dismissed:** [count]
**Changelog entries added:** [count]
```

### 9. Surface follow-ups

If a pattern suggests a deeper rethink ("step 5 of cycle-protocol.md needs restructuring", "we need a new protocol for X"), surface it as a strategic question to the user — do not silently rewrite.

## Constraints

- **Proposed-only.** No `system/` change without explicit user yes. No exceptions.
- **`changelog.md` is mandatory.** Every applied change logged there per `cycle-protocol.md` § 5c.
- **Prefer a recoverable delete** (a trash tool like the `trash` CLI if available; otherwise `rm` — git keeps every prior version).
- **Touch only `system/`** (plus the dream log entry and changelog). Instance cleanup routes to `dream-instance`.
- **Surface contradictions, don't reconcile.** Same cognitive discipline rule as everywhere else.
- **Cross-deployment safety.** Anything in `system/` is portable across deployments — never inject project-specific content here. If a candidate addition is project-specific, route it to `instance/rules.md` instead.

## Output

A leaner `system/`; a `changelog.md` entry per applied change; a dream log entry; a short verbal summary to the user.
