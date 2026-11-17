# Gemini and Antigravity-Specific Instructions

Supplement to `AGENTS.md`. Apply strictly when running in Gemini CLI or Antigravity.

## Model Hierarchy & Roles
Always use the latest available version following: **Carbon > Argon > Hydrogen** (always prioritizing currently available models).

- **Carbon & Argon (Orchestration & Planning):** Responsible for analyzing requests, designing architecture, and creating the execution plan to delegate to **Hydrogen** (explicitly assigning Hydrogen's reasoning level). Select **Carbon** for high-complexity/critical tasks and **Argon** for standard analysis and planning, subject to availability.
- **Hydrogen (Implementation & Execution):** Default sub-agent for all code implementation, bug fixes, tests, documentation, and Git merge/cleanup tasks triggered by the orchestrator.
  - *Effort:* `high` by default; adjust to `low`, `medium`, or higher reasoning levels as delegated by Carbon/Argon based on task complexity.

## Escalation on Failure
- If **Hydrogen** fails verification, retry **once** with a clearer plan or adjusted reasoning level.
- If it fails a second time, escalate the task to **Argon** or **Carbon** depending on difficulty and model availability.