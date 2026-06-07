# Glossary

**User** — The human operating Claude Code.

**Agent** — A role-based AI persona with a defined identity, responsibilities, and artifact ownership. Technically, the main Claude Code assistant adopts the agent's role.

**Subordinate Agent** — An agent that reports to another agent. Spawned via Claude Code's Agent tool. Runs in its own context window. Has its own identity, state-of-mind, artifacts, and skills.

**Delegation** — When a parent agent spawns a subordinate for specialized execution. Context is forked, not shared. The subordinate returns a concise summary; raw data stays in its own artifacts.

**Skill** — A context loader: a pre-packaged knowledge file pulled into conversation to give domain expertise on demand. Lives in each agent's `skills/` folder.

**Cycle** — A single conversation/session. The fundamental unit of work. Follows the cycle protocol.

**Standing Artifact** — A persistent file that always exists and reflects current reality. Expands and contracts with scope. Examples: `state-of-mind.md`, `strategy.md`, `recurring-tasks.md`.

**Project File** — A working document in `projects/` tied to a specific initiative. Has a lifecycle: created when work starts, deleted when done. Named `yyyy-mm-<descriptive-name>.md` with frontmatter for fast discovery.

**Intelligence File** — A living document in `instance/agents/[agent-name]/intelligence/` that captures durable knowledge about how the world works — external principles, market insights, competitive knowledge, domain expertise. Distinguished from strategy (what we'll do) and projects (work in progress). Intelligence says "this is how things work; factor it in." Must be actively curated: stale or contradicted intelligence gets deleted, not kept. Intelligence is a hypothesis, not a fact — it informs reasoning but does not replace it. See Cognitive Discipline.

**Cognitive Discipline** — The four cognitive traits every agent must embody: epistemic humility (holding beliefs as working hypotheses), multi-perspectival thinking (examining problems from multiple angles), dialectical reasoning (holding contradictions in tension instead of prematurely resolving them), and steel-manning (building the strongest version of opposing arguments). Operationalized through data-first analysis, mandatory contradiction surfacing, and periodic intelligence challenges. Defined in `CLAUDE.md` → Cognitive Discipline, operationalized in `system/agent-rules.md` → Cognitive Discipline.

**Anchoring Bias** — The cognitive vulnerability where information loaded early in context disproportionately shapes all subsequent reasoning. In Agent OS, intelligence files loaded during initialization create anchoring risk. The data-first rule and contradiction surfacing mechanisms exist specifically to counter this. When an agent's reasoning follows the pattern "intelligence says X, therefore X" — that is anchoring, not analysis.

**Strategy Session** — A special cycle dedicated to creating or revising strategy docs. User-initiated.

**Impulse Check** — The proactive step at cycle start where the agent scans for overdue, stalled, or high-priority work and proposes what to do next.

**Agent OS** — The persistent multi-agent framework itself: the folder structure, artifacts, knowledge files, agent definitions, and protocols that collectively power all work. Synonym: agentos.

**Meta-programming** — Edits and improvements to the agentic system itself — adding new agent roles, editing flows, adding or removing rules, restructuring protocols, changing how agents interact. Distinguished from "execution work" (what agents do within the system) vs "meta work" (changing the system itself).
