# Claude Code-Specific Instructions

Supplement to `AGENTS.md` or `CLAUDE.md`. Apply strictly when running in Claude Code or Claude Desktop.

## Model Hierarchy & Roles
Always use the latest version following: **Fable > Opus > Sonnet > Haiku**.

- **Fable:** Orchestrator, planning, and user consultation only. Never delegate coding or git tasks to Fable.
- **Opus:** Complex architecture, critical refactors, and high-risk debugging.
  - *Effort:* `low` or `medium` by default; `high` only for extreme edge cases (`xhigh`/`max` forbidden).
- **Sonnet:** Standard feature implementation, bug fixes, tests, and codebase research.
  - *Effort:* `medium` or `high` based on complexity (`low` for simple bounded tasks; `xhigh`/`max` forbidden).
- **Haiku:** Minor code tweaks, formatting, documentation, and executing Git merge/cleanup tasks triggered by the orchestrator.
  - *Effort:* Unrestricted (`low`, `medium`, `high`, `xhigh`, or `max` as needed).

## Delegation
- **Claude Desktop:** Delegate each implementation task (feature, fix, test round) to a **linked session** (`start_session`, initiation `user_asked`, `model` per the hierarchy above) so it appears nested in the sidebar and can be opened or steered. Treat this rule as the user's explicit request for sessions. Reuse the same session (`SendMessage`) when iterating on its task.
- **Sub-agents (`Agent` tool):** Only for read-only research, or as a fallback where linked sessions are unavailable (Claude Code CLI).

## Escalation on Failure
- If a session or sub-agent produces an inadequate result or fails verification, retry **once** with a clearer brief or higher allowed effort.
- If it fails a second time, escalate one tier up the implementation ladder (`Haiku -> Sonnet -> Opus`), never to Fable.

## Worktrees
- **Isolation:** Isolate parallel write tasks in separate worktrees, one per task branch. In Claude Desktop, let each linked session create its own worktree (`use_worktree: true`, `source_branch: development`, `branch_name: <prefix>/<task>`). Only where linked sessions are unavailable (Claude Code CLI, sub-agents), create them manually at `~/.worktrees/<project_name>/<worktree_name>`. Never create worktrees for read-only tasks.
- **Cleanup:** After merging a session's branch into `development`, archive the session to remove its worktree instead of deleting it manually; still delete the local task branch.