# Gemini and Antigravity-Specific Instructions

Supplement to `AGENTS.md`. Apply strictly when running in Gemini CLI or Antigravity.

## Model Hierarchy & Roles
Always use the latest available version following: **Neon > Carbon > Argon > Hydrogen** (always prioritizing currently available models).

- **Neon (Primary Orchestrator & Thinking):** Default model for the main session. Analyzes requests, plans, delegates work to sub-sessions/sub-agents, verifies results, triggers Git merge/cleanup, and reports completion briefly to the user. If Neon is unavailable, fall back to **Carbon -> Argon** for orchestration.
- **Carbon (High-Complexity Specialist):** Use for critical architecture, extreme edge cases, or heavy debugging when available.
- **Argon (Fallback & Quota Buffer):** Intermediate fallback for escalated implementation or orchestration (separate quota pool from Neon/Hydrogen).
  - *Effort:* `medium` by default; the orchestrator adjusts it dynamically based on task complexity.
- **Hydrogen (Primary Implementation & Execution):** Default sub-agent for code implementation, bug fixes, tests, documentation, and Git merge/cleanup tasks.
  - *Effort:* `high` by default for code; the orchestrator adjusts it dynamically (e.g., `low` or `medium` for minor edits, docs, or Git cleanup).

## Orchestration, Closure & `/swarm`
- **Task Closure Flow:** The orchestrator delegates implementation to **Hydrogen** -> verifies that the outcome meets requirements -> invokes **Hydrogen** (low reasoning) to run the merge, push, and cleanup -> closes the task and notifies the user briefly.
- **Parallelism & `/swarm`:** Use standard sub-sessions/sub-agents for everyday tasks to preserve weekly quotas. Reserve `/swarm` for large, complex, or critical tasks where parallel cross-verification justifies higher token usage (or when explicitly requested).

## Escalation on Failure
- If **Hydrogen** fails verification, retry **once** with a clearer plan or adjusted reasoning level.
- If it fails a second time, escalate progressively by availability to preserve Neon's quota: **Hydrogen -> Argon -> Carbon -> Neon**.