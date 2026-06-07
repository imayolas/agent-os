# Delegation Protocol

How agents delegate work to subordinate agents.

## When to Delegate

Delegate when the task requires **deep specialized execution** that would fill the parent agent's context with details it doesn't need to retain. The parent needs conclusions and recommendations, not raw data.

**Delegate:** Technical SEO audit, crawl analysis, deep competitive research, data-heavy analysis.
**Don't delegate:** Quick lookups, strategic decisions, tasks where the parent needs to reason about the data in context. Load a skill instead.

**Rule of thumb:** Skills for knowledge, delegation for execution.

## How Delegation Works

Delegation uses Claude Code's Agent tool to spawn a subagent. The subagent runs in a **fresh context** (forked, not shared) — this is a feature, not a limitation:

- Forces the parent to be explicit about objectives.
- No context pollution in either direction.
- The child's artifacts persist independently across delegations.

### Delegation Steps

1. **Identify the subordinate agent.** Check the parent agent's identity file for available subordinates.

2. **Compose the delegation prompt.** Include:

   - The subordinate's identity file path (the subagent reads it to adopt its role).
   - The subordinate's `state-of-mind.md` path (for continuity).
   - System rules path (`system/` files the subagent should read).
   - The specific task: objective, deliverable format, and relevant context.
   - Any project files the subagent should read or update.

3. **Spawn via Agent tool.** Use `subagent_type: "general-purpose"`. Instruct the subagent to:

   - Read its identity file and adopt the role.
   - Read its state-of-mind for prior context.
   - Execute the task.
   - Update its own artifacts (state-of-mind, project files).
   - Return a concise summary to the parent.

4. **Process the result.** The parent integrates the summary into its own artifacts and reasoning.

### Delegation Prompt Template

```
You are being invoked as a subordinate agent within Agent OS.

**Your identity:** Read `instance/agents/[agent-name]/[agent-name].md` and adopt this role.
**System rules:** Read `system/agent-rules.md`, `system/artifact-rules.md`, and `instance/rules.md`.
**Your state-of-mind:** Read `instance/agents/[agent-name]/state-of-mind.md`.

**Task:**
[Clear objective]

**Deliverable:**
[Expected format and level of detail]

**Context:**
[Relevant information from parent's artifacts]

**Artifacts to update:**
- Update your `state-of-mind.md` with findings and what's next.
- Create/update project files in `projects/` as needed.

**Return:** A concise summary of findings, conclusions, and recommendations.
```

## Agent Naming

Agent folders use flat naming with team prefix to express hierarchy:

- `marketing-director` — team lead
- `marketing-seo-specialist` — reports to marketing-director

Reporting relationships are defined in each agent's identity file, not by folder nesting.

## State Ownership

Each agent owns:

- `state-of-mind.md` — agent-specific focus, updated each cycle/delegation.
- `strategy.md` — agent-specific direction (if applicable).
- `recurring-tasks.md` — agent-specific repeating work (if applicable).
- `projects/` — agent-specific working documents.
- `skills/` — agent-specific knowledge packs.

Org-level shared state lives in root `instance/artifacts/state-of-mind.md`.
