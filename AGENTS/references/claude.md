# Claude Code-Specific Instructions

These instructions supplement the common global instructions. Apply them only when the session is running in Claude Code.

## Orchestration Strategy

- For substantial tasks, act as an orchestrator: plan, decompose, delegate implementation, review results, and verify integration.
- Keep the main context lean. Delegate bulk exploration or implementation when a sub-agent can perform it effectively.
- Handle conversation, quick answers, and very small edits directly.
- Keep architectural, security-sensitive, and ambiguous decisions in the main agent; delegate mechanical implementation.

## Model Selection

- Model ladder: Fable 5.1 > Opus 5.5 > Sonnet 5.5 > Haiku 5.5.
- By default, use sub-agents strictly below the orchestrator's model.
- Choose each sub-agent's model and effort according to complexity, risk, and ambiguity.
- For sub-agents, use only `low`, `medium`, or `high` effort. Pass effort explicitly when supported. Never use `xhigh` or `max`.

## Selection Guidelines

When the orchestrator is Opus 5.5:

- Sonnet 5.5, high: complex features, refactors, or difficult debugging.
- Sonnet 5.5, medium: standard implementation, fixes, tests, or research.
- Sonnet 5.5, low: small, well-specified edits requiring judgment.
- Haiku 5.5: trivial edits, formatting, boilerplate, or simple lookups. Use the lowest effort that fits.

For other orchestrators:

- Fable 5.1 may delegate to Opus 5.5 or lower.
- Sonnet 5.5 delegates only trivial tasks to Haiku 5.5.
- Haiku 5.5 works directly.

## Delegation and Review

- Launch independent sub-agents together when useful. Never run concurrent writers on the same files.
- Request short reports of changes and verification, not file dumps.
- Verify delegated changes, relevant checks, and integration before marking the task complete.
- If a lower-tier result is inadequate, retry once with a clearer brief, then escalate to the orchestrator's tier when supported.

## Explicit Opt-Out

- If the user explicitly requests no lower-tier sub-agents, work directly or use same-model, same-quality sub-agents.
- This does not override an explicit request to avoid all delegation.
- Apply the preference to the current request unless told otherwise.

## Verification Expectations

- Aim for zero linter/analyzer violations and build errors or warnings.
- Run the relevant test suite and follow project-specific QA rules.
- Report existing failures and blocked checks honestly.
