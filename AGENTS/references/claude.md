# Claude Code-Specific Instructions

Supplement to `AGENTS.md`. Apply strictly when running in Claude Code.

## Model Hierarchy & Roles
Always use the latest version following: **Fable > Opus > Sonnet > Haiku**.

- **Fable:** Orchestrator, planning, and user consultation only. Never delegate coding or git tasks to Fable.
- **Opus:** Complex architecture, critical refactors, and high-risk debugging.
  - *Effort:* `low` or `medium` by default; `high` only for extreme edge cases (`xhigh`/`max` forbidden).
- **Sonnet:** Standard feature implementation, bug fixes, tests, and codebase research.
  - *Effort:* `medium` or `high` based on complexity (`low` for simple bounded tasks; `xhigh`/`max` forbidden).
- **Haiku:** Minor code tweaks, formatting, documentation, and executing Git merge/cleanup tasks triggered by the orchestrator.
  - *Effort:* Unrestricted (`low`, `medium`, `high`, `xhigh`, or `max` as needed).

## Escalation on Failure
- If a sub-agent produces an inadequate result or fails verification, retry **once** with a clearer brief or higher allowed effort.
- If it fails a second time, escalate one tier up the implementation ladder (`Haiku -> Sonnet -> Opus`), never to Fable.