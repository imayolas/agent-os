# Instance Rules

Business rules specific to **this** deployment. These constrain agent behavior based on your project's context. For generic, project-agnostic behavioral rules, see `system/agent-rules.md`.

> This file ships with a generic placeholder. Replace it with your own deployment's rules during onboarding (run `/onboard`). Delete sections that don't apply and add the ones that do.

## Product / Domain Context

_What does this deployment work on? If your agents operate on a codebase, dataset, or external system, describe it here so they know where the ground truth lives (see "Source-First Research" in `system/agent-rules.md`)._

Example:

- `path/to/code` — the product the agents reason about.
- `path/to/data` — datasets agents may query.

## Domain-Specific Constraints

_Any rules unique to your business or domain: priority ordering across workstreams, audiences, timing/seasonality, compliance constraints, tone, etc. Remove if not applicable._

## Approval & Publishing

_Who signs off before work goes live? Where does published output land? Define it here so agents don't guess._
