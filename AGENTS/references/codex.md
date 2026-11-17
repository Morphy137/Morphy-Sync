# Codex-Specific Instructions

Supplement to `AGENTS.md`. Apply strictly when running in Codex.

## Model Hierarchy & Roles
Always use the latest available version following: **Astra > Sol > Terra > Luna**.

- **Astra:** High-level orchestration, consultation, or critical architectural decisions only.
  - *Effort:* Strictly `low` or `medium` (`high`, `xhigh`, and `max` forbidden).
- **Sol:** Main session default and complex implementation, architecture refactors, or difficult debugging.
  - *Effort:* `medium` by default; `high` for complex tasks (`xhigh`/`max` forbidden).
- **Terra:** Default sub-agent for standard code implementation, features, bug fixes, and tests.
  - *Effort:* `high` by default; adjust to `low`, `medium`, `xhigh`, or `max` depending on task complexity.
- **Luna:** Concrete minor edits, documentation, boilerplate, and executing Git merge/cleanup tasks triggered by the orchestrator.
  - *Effort:* Unrestricted (`low`, `medium`, `high`, `xhigh`, or `max` as needed).

## Escalation on Failure
- If a sub-agent fails verification, retry **once** with a clearer brief or higher allowed effort.
- If it fails a second time, escalate one tier up (`Luna -> Terra -> Sol`). Do not automatically escalate to **Astra** without user authorization.