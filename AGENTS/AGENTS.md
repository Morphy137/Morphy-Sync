# Global Agent Instructions

## Tool-Specific Reference
Identify the active client harness (not from the model name alone) and read **only** its reference file relative to this directory before planning or executing work:
- Codex: `references/codex.md`
- Claude Code: `references/claude.md`
- Gemini CLI / Antigravity: `references/gemini.md`

## Orchestration, Sub-Sessions & Worktrees
- **Lean Main Session:** Use the main session strictly to analyze requests, confirm scope, and orchestrate. Delegate features, fixes, and testing to sub-sessions/sub-agents. Reuse existing sub-sessions when iterating on the same task. Require short summary reports back, never file dumps.
- **Context Compaction:** Automatically compact context at **90% capacity**, strictly between tasks (after finishing a task or before starting a new one, never mid-implementation).
- **Mandatory Sub-Agent Briefing:** Always pass project context to sub-agents and explicitly instruct them to read `AGENTS.md` and obey core constraints (strictly **no `Co-authored-by`**, English commits, targeted tests only).
- **Worktree Isolation:** Isolate parallel write tasks in separate worktrees at `~/.worktrees/<project_name>/<worktree_name>` (one per task branch). Never create worktrees for read-only tasks.

## Configuration — AgentSync
Always load and follow the `agentsync` skill when installing, updating, or syncing skills, MCP servers, or agent configs instead of editing client files manually.

## Git & Version Control
- **Branches:** Keep only `main` (releases) and `development` (integration) as permanent branches. Branch every task from `development` using `feature/`, `fix/`, `refactor/`, or `chore/` prefixes.
- **Commits & Authorship:** Make incremental commits per logical change using **Conventional Commits in English**. Never add AI/tool attribution or `Co-authored-by` trailers anywhere.
- **Delegated Auto-Merge & Cleanup:** Once task implementations are verified, the orchestrator must invoke a sub-agent to **merge the task branch into `development`, run `git push origin development`, and delete the local task branch and temporary `git worktree`** (unless instructed otherwise).
- **Restrictions:** Never commit, merge, or push to `main`, and never force-push without explicit user authorization. Never commit secrets, `.env` files, or private keys.

## Code, Architecture & i18n
- Follow **Clean Code** and **Clean Architecture**. Write all code identifiers, comments, and commits in **English**.
- Reuse existing project patterns and libraries before creating new ones. Ask before adding heavy dependencies.
- Comment only non-obvious logic or constraints; never comment obvious code.
- **i18n:** Never hardcode user-facing strings. Use the project's localization dictionaries with **English (`en`)** by default plus any user-requested languages.

## Verification & Documentation
- **Targeted Tests:** Run **only** tests/linters directly related to modified code to save tokens. Run full suites/builds only on major structural changes. Stop and report options after 3 failed attempts on the same error.
- **Context7:** Query live docs (`npx ctx7@latest library <name> "<query>"` -> `npx ctx7@latest docs <libraryId> "<query>"`) **only** for new tools, recent APIs, or version mismatches.

## Communication
- **Ultra-Brief:** Be direct and concise. Report only key outcomes, critical decisions, or essential caveats. No step-by-step narration or long explanations unless requested. Ask with a brief proposal before making breaking structural changes.