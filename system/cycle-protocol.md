# Cycle Protocol

A cycle is a single Claude Code conversation. It is the fundamental unit of work in Agent OS.

## Cycle State File

**Path:** `instance/cycles/state` — single-line coordination signal to prevent double-opens and double-closes across parallel Claude Code instances.

**Format:** `cycle_number:status:agent:date` (e.g., `12:open:ceo:2026-03-09`)

**Status values:** `open` | `closed`

## Starting a Cycle

A cycle begins when the user invokes an agent, typically with:

> "New cycle for [Agent Name]"

**Before opening, read `instance/cycles/state`:**

- If `open` → a cycle is already running. You are joining it. Use the existing cycle number. Do NOT write a new state or re-initialize.
- If `closed` → increment the cycle number, write `N:open:agent:date` to `instance/cycles/state`, then proceed with initialization.

Optionally, the user provides instructions. If not, the agent runs an impulse check to decide what to do.

## Cycle Steps

These steps can overlap, reorder, or run in parallel. But every cycle must include all of them.

### 1. Initialize

- Read the agent's identity file: `instance/agents/[agent-name]/[agent-name].md`.
- Read the agent's `state-of-mind.md` to recover current focus.
- Read `instance/artifacts/state-of-mind.md` for org-level context.
- Scan `instance/agents/[agent-name]/intelligence/` — read frontmatter (first 10 lines) of each intelligence file. Load full files relevant to this cycle's work. Intelligence files contain durable knowledge that must inform decisions.
- Scan `instance/agents/[agent-name]/projects/` — read frontmatter (first 10 lines) of each project file to understand active work. Load full files only as needed.
- Read `instance/agents/[agent-name]/strategy.md` and `instance/agents/[agent-name]/recurring-tasks.md` if they exist.
- Load skills from the agent's `skills/` folder as needed.
- **Environment check (mandatory, blocking):** Before any execution, verify that every tool and authenticated session needed for this cycle is actually working. If anything is missing, surface all missing prerequisites in a single message and wait for confirmation.

### 2. Decide (Impulse Check)

If the user provided instructions, follow them. Otherwise:

- Check `recurring-tasks.md` for overdue or due tasks.
- Review `state-of-mind.md` for stalled or time-sensitive items.
- Scan project file frontmatter for unblocked work.
- Propose a prioritized short list. Let the user approve or redirect.

### 3. Execute

**Before doing any task yourself:** Check if it falls within a subordinate agent's responsibilities. If it does, delegate — don't do it directly.

**Data-first rule (mandatory for analysis work):** When analyzing data — GSC, GA4, competitive research, customer feedback, market signals — form your own conclusions from the raw evidence _before_ consulting intelligence files. Write down what the data says. Then open intelligence and cross-reference. If they diverge, that divergence is the most valuable signal in the cycle. See `CLAUDE.md` → Cognitive Discipline.

Do the work:

- Research, analysis, content creation, or any domain work.
- Delegate to subordinate agents for specialized execution.
- Produce tangible outputs.
- Update project files with progress and results.

### 4. Reflect

- Rewrite the agent's `state-of-mind.md` with current focus, open questions, and what's next.
- Rewrite `instance/artifacts/state-of-mind.md` with the org-level picture: active agents, cross-agent priorities, shared context.
- **Intelligence update:** Review intelligence files touched by this cycle's work. Did you learn anything new? Did any existing intelligence become outdated or contradicted? Update, add, or delete accordingly. Stale intelligence leads to generic, wrong decisions — curate aggressively.
- Update any project files that changed.
- Delete project files for finished work (`status: done`).
- **Strategy drift check:** Does this cycle's work align with strategy? If not, flag it.

### 5. Meta-Improve

Self-improvement is a first-class function of the system. It is how Agent OS avoids stagnation. Every cycle must include this step — it is not optional, not "if time permits."

#### 5a. Cognitive discipline check (mandatory)

- Did I anchor on prior intelligence instead of reasoning from data this cycle?
- Did I dismiss or explain away contradictory evidence?
- Did I analyze from only one perspective when multiple were available?
- If this is a strategy session or every 5th cycle: actively challenge intelligence entries. Argue against each one using current data. Update or kill entries that don't survive.

If the answer to any of the first three is yes, flag it in the cycle summary. This is a system failure, not a minor note.

#### 5b. Process reflection

Evaluate the cycle's _process_, not just its output:

- What felt inefficient or unclear?
- Did any protocol step cause friction or confusion?
- Was any information missing at initialization that should have been available?
- Were there repeated manual steps that could be codified as a skill or recurring task?

#### 5c. Improvement actions

**Threshold rule:** Changes to `system/` files (the engine) are **always proposed to the user**, never applied silently. The risk of silent drift in the framework is higher than the cost of asking. Changes to `instance/` files (skills, recurring tasks, agent definitions) may be applied directly when they are clearly beneficial and low-risk.

For every proposed or applied improvement:

1. State what changed and why.
2. Log the change in `system/changelog.md` (for system changes) or in the cycle log entry (for instance changes).
3. In the **next cycle's initialization**, check: "Did last cycle's system change improve things?" — one sentence in the cycle summary. This closes the feedback loop.

### 6. Close

**Before closing, read `instance/cycles/state`:**

- If already `closed` → another instance already closed this cycle. Do NOT write a duplicate log entry. Skip to announcing completion.
- If `open` → proceed: write the cycle log entry to `instance/cycles/log.md`, then write `N:closed:agent:date` to `instance/cycles/state`.

- Announce cycle completion with a brief summary and what's queued next.

## Delegated Cycles

When a cycle runs as a background/delegated agent (subagent, fork, worktree):

- Steps 1–3 (Initialize, Decide, Execute) run normally.
- **Before steps 4–6 (Reflect, Meta-Improve, Close):** return findings, insights, and proposed artifact changes to the principal (user or parent agent). Do not persist until approved.
- The principal may adjust, reject, or approve before the cycle closes.

This prevents delegated agents from writing conclusions that haven't been reviewed — especially important for strategy work where the user's business judgment must override data-only recommendations.

## Cycle Log Entry Format

Newest entries at the top of `instance/cycles/log.md`:

```
## Cycle [number] — [short name]
**Date:** YYYY-MM-DD
**Agent:** [agent name]

**Summary:** One or two sentences on what was accomplished.

**Artifacts changed:**
- [artifact path]: [what changed]

**Next:** What's queued or suggested for the next cycle.
```
