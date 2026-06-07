# System Changelog

Tracks modifications to the framework (`system/` files and `CLAUDE.md`). Every change must include a date, what changed, and why.

This file is mandatory: agents may not modify `system/` files without logging the change here.

---

## 2026-06-07 — Example squad + squad-oriented onboarding

**What changed:**

- Replaced the solo `example-researcher` agent with a generic **example marketing squad**: a Marketing Director (lead) that delegates to a Content Writer, SEO Analyst, and PPC Expert. Includes a worked example project showing the delegate → integrate loop, an example strategy doc, and an example intelligence file.
- Reframed `CLAUDE.md`'s example-agent language and `instance/artifacts/state-of-mind.md` around the org model: **the user is the CEO**, the lead reports to them, specialists report to the lead.
- Rewrote the `onboard` skill to teach squad-based delegation (CEO → lead → specialists) and to interview the user into designing a *team*, not a single agent.
- Updated the README with the CEO/lead/specialist model and an org-chart diagram.

**Why:** A single "researcher" agent is a cliché that hides the framework's core value — squad-based, delegated multi-agent work. The example now demonstrates the org chart and delegation flow that make Agent OS more than a stateful chatbot, and reinforces that the human operates as CEO.

---

## 2026-06-07 — Public-release sanitization

**What changed:**

- Removed the private instance (a marketing org for one specific product) and replaced it with a generic example agent (`example-researcher`) plus an empty, `.gitkeep`-backed skeleton.
- Rewrote all internal paths from the `agent-os/`-prefixed monorepo layout back to the standalone repo root (`instance/`, `system/`, `bin/`, `CLAUDE.md`). The repo had been copied out of a host monorepo without updating paths.
- Genericized the framework engine: the "Codebase-First Research" rule became "Source-First Research" (no product-specific example), and product-name references were removed from system files.
- Authored the `onboard` skill and the previously-missing `dream-instance` / `dream-system` skills under `.claude/skills/`.
- Sanitized `.mcp.json` into `.mcp.json.example` and gitignored the real config.
- Standardized the project name on "Agent OS" and rewrote the README.
- Removed the "Instance Health Check (Do This First)" section from `CLAUDE.md`. It was a relic of an rsync-based persistence model where `instance/` could silently lose data; with git-tracked state the file-count tripwire no longer earns its place.

**Why:** Preparing the framework to be shared publicly as a standalone, project-agnostic starter. The goal is that a fresh clone runs cleanly with no dead paths, no private data, and a guided onboarding entry point.

---

## 2026-05-27 — Dream skills added; periodic system review deleted

**What changed:**

- Added `.claude/skills/dream-instance/SKILL.md` and `.claude/skills/dream-system/SKILL.md`. Monthly cross-cycle consolidation passes over `instance/` and `system/` respectively, biased toward forgetting, with mandatory reality-check against the actual codebase and interactive user confirmation on every deletion.
- Deleted §5d ("Periodic system review every 10 cycles") from `system/cycle-protocol.md`. Its four responsibilities (changelog evaluation, worked-around rules, boundary violations, structural improvements) are fully subsumed by `dream-system`, which adds reality-check, calendar-based cadence, and forgetting bias on top.
- Added a "Dream Cadence Check" section to `CLAUDE.md` so cycles nudge the user when a monthly dream is overdue. Quick Start updated to document `/dream-instance` and `/dream-system` invocation.

**Why:** Inspired by Anthropic's Dreaming feature for Claude Managed Agents — scheduled between-session memory consolidation. Agent OS already had per-cycle meta-improve (§5a–c) for within-cycle reflection by the active agent, but no cross-cycle, cross-agent consolidation step beyond the cycle-count-triggered §5d. §5d lacked a reality-check, lacked a calendar cadence, and had no forgetting bias. The dream skills replace it with a calendar-triggered, codebase-grounded pass that actively encourages compression and deletion. §5a–c remain because they are per-cycle reflections the dream skills cannot perform retroactively.

---

## 2026-03-19 — Cycle continuation and codebase-first rules

**What changed:**

- Added "Cycle Continuation = Full Re-initialization" rule to `system/agent-rules.md`. When continuing an existing cycle, agents must run the full init checklist, not a partial load.
- Added "Codebase-First Research" rule to `system/agent-rules.md`. Agents must analyze the product codebase before asking the user about product behavior.

**Why:** In Cycle 17, the agent skipped the init checklist when continuing a cycle from a different conversation, which caused it to fall back to conversational patterns ("should I do X?") instead of autonomous operation. The agent also asked the CEO to explain product behavior that was discoverable in the codebase. Both rules codify the corrections made during that cycle.

---

## 2026-03-19 — Migration to monorepo

**What changed:**

- Moved Agent OS from a standalone repo into a host product monorepo at `agent-os/`.
- All internal paths updated to be relative to monorepo root (prefixed with `agent-os/`).
- Updated `instance/rules.md` to reference product codebase with relative paths instead of absolute.
- Root `CLAUDE.md` updated to reference `CLAUDE.md` for cycle invocations.

**Why:** Having Agent OS in a separate repo created friction — marketing cycles needed to touch files in the product repo, requiring cross-repo context and two working directories. Co-locating agent-os with the product codebase it operates on eliminates this friction.

---

## 2026-03-19 — Self-improvement overhaul

**What changed:**

- Rewrote Meta-Improve (step 5) in `system/cycle-protocol.md` from a brief checklist into a structured protocol with four sub-steps: cognitive discipline check, process reflection, improvement actions with threshold rules, and periodic system review.
- Added threshold rule: changes to `system/` files must always be proposed to the user, never applied silently.
- Added feedback loop closure: next cycle checks whether the previous cycle's system change actually helped.
- Added periodic system review every 10 cycles for deeper framework health checks.

**Why:** The previous meta-improve was underspecified — three bullet points with no enforcement mechanism. System files could be silently modified, there was no way to evaluate whether changes helped, and the "small fixes: apply directly" threshold was undefined. Self-improvement is a critical function that needs structure proportional to its importance.

---

## 2026-03-19 — Layer separation

**What changed:**

- Split the repo into `system/` (portable engine) and `instance/` (project-specific state).
- Moved all agents, artifacts, cycles, tools, design-explorations into `instance/`.
- Extracted instance-specific business rules from `system/agent-rules.md` → `instance/rules.md`.
- Extracted business-specific glossary terms from `system/glossary.md` → `instance/glossary.md`.
- Moved `system/mcp-setup.md` → `instance/mcp-setup.md`.
- Rewrote `CLAUDE.md` as a pure framework bootstrap with separate system/instance references.
- Created this changelog.

**Why:** The framework (how agents work) and the instance (what agents work on) were entangled. Business rules leaked into system files, making the engine non-portable. This separation enables reuse across projects and clearer ownership of each layer.
