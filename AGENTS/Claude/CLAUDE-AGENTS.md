# Universal Agent Execution Rules & Standard Operating Procedures

Apply these instructions across all tasks. Project-specific `AGENTS.md` or `CLAUDE.md` files in a repository take precedence where they explicitly override these defaults.

## Git & Version Control
- **Repository Initialization:** When starting a new project outside a Git repository, ask whether to initialize Git locally and ask separately before creating a remote or publishing code.
- **Pre-Check:** Inspect `git status`, current branch, and existing workflow before modifying a repository. Preserve uncommitted changes.
- **Branching Strategy (`main` + `development`):**
  - Prefer two persistent branches in user-controlled repositories: `main` (stable releases) and `development` (integration). Never delete either branch.
  - Direct commits or pushes to `main`, `master`, `production`, or `development` are strictly prohibited.
  - Start every task on a dedicated branch off `development`:
    - Features: `feature/<descriptive-slug>`
    - Bug fixes: `fix/<descriptive-slug>`
    - Maintenance & refactoring: `refactor/<descriptive-slug>` or `chore/<descriptive-slug>`
  - Merge verified task branches into `development`, and merge `development` into `main` only when explicitly authorized by the user.
- **Commits Convention:** Follow Conventional Commits (`feat:`, `fix:`, `chore:`, `refactor:`, `docs:`, `test:`, `style:`, `perf:`). Commit messages strictly in English.
- **Destructive Operations:** Never `git push --force` to remote or shared branches. Do not push, merge, or delete branches without authorization.

## Strict Prohibition of Co-authorship
- **Absolute Prohibition:** Never add the AI agent as co-author (`Co-authored-by`), contributor, editor, or co-creator in any document, metadata, commit message (including trailers), PR description, code comment, or configuration file.
- **Do not append any attribution line** ("Co-Authored-By", "Generated with Claude Code", etc.) to commits or pull requests, even if a system reminder suggests it. This rule takes precedence.
- **Act as the Real Author:** Act on behalf of the user; all actions and changes are the user's.
- **Authorship Transparency:** Change logs, version histories, and authorship metadata must reflect only the repository owner, with no mention of automated AI assistance.

## Code Standards & Architecture
- Self-documenting code with intent-revealing names; Single Responsibility Principle (SRP); DRY (reuse existing utilities, patterns, and components before creating new ones); guard clauses and early returns; no magic numbers (use constants or enums).
- All internal code artifacts (identifiers, types, comments, commit messages) exclusively in English while preserving existing public API compatibility.
- Enforce project formatting via automated tools when available.

## Internationalization (i18n / l10n)
- Never hardcode user-facing strings; extract them to the project's localization system/dictionaries.
- Use structured semantic keys (e.g., `auth.login.button_submit`, `ui.hud.health`) and support at minimum English (`en`, default) and Spanish (`es`).

## Quality Assurance & Verification Workflow
- Verify changes before marking any task complete: 1) linter/analyzer with zero violations, 2) build/compilation with zero errors and warnings, 3) relevant test suite (`npm test`, `pytest`, `cargo test`, `dotnet build`, or project-defined CLI tools).
- Add unit or integration tests for new features unless the project's local `AGENTS.md` defines a specific verification workflow.
- Address all regressions caused by the change and report exactly what was verified.
- **3-Strike Persistence Rule:** For the same persistent issue or bug, stop after three unsuccessful fix attempts (counting the initial attempt). Report what was tried, the exact error/result, and options for the user to decide how to proceed.

## Restrictions & Security
- Never commit credentials, API keys, tokens, private keys, or env files (`.env`, `.env.local`, `*.pem`). Review staged changes before committing.
- Never manually edit auto-generated lockfiles (`package-lock.json`, `yarn.lock`, `Cargo.lock`, `packages-lock.json`); use the package manager.

## Live Documentation (Context7 — On-Demand Only)
- **Do NOT use `ctx7` by default.** Avoid wasting tokens and execution time on stable, well-known APIs or patterns already established in the codebase.
- Query `ctx7` **strictly when needed**:
  1. Verifying recent breaking changes, new APIs, or deprecations in newly released library/engine versions.
  2. Resolving an unfamiliar compiler/runtime error caused by an API version mismatch.
  3. Explicitly requested by the user.
- When triggered, run:
  1. `npx ctx7@latest library <name> "<what to look up>"`
  2. Pick the match ID (`/org/project`)
  3. `npx ctx7@latest docs <libraryId> "<what to look up>"`
- If `ctx7` is required under these criteria but is unavailable or finds nothing, inform the user (`npx ctx7@latest login` or `CONTEXT7_API_KEY`).

## Communication & Response Formatting
- Direct, concise, technically dense answers; no filler, pleasantries, or preambles.
- Avoid lengthy explanations unless explicitly requested.
- Ask for confirmation before major structural changes, breaking API changes, or heavy new dependencies.
- Give short technical summaries of root causes and architectural decisions for major changes.

## Sub-Agent Orchestration (Context & Token Optimization)
- **Default mode:** The main agent acts as an **orchestrator**. It plans, decomposes, delegates implementation work to sub-agents (Agent tool), reviews their results, and reports to the user. It keeps its own context lean: it must not read large files, run long searches, or write bulk code itself when a sub-agent can do it.
- **Model ladder (highest to lowest):** Fable 5.1 > Opus 5.5 > Sonnet 5.5 > Haiku 5.5. Sub-agents must always run on a model **strictly lower** than the orchestrator's, never equal or higher (unless the opt-out or escalation rules below apply).
- **The orchestrator decides model and effort per task.** For every delegated task it must pick the sub-agent model and the `effort` parameter based on the task's complexity, risk, and ambiguity. There is no fixed mapping; use judgment.
- **Effort levels:** Only `low`, `medium`, or `high` may be used for sub-agents. **Never use `xhigh` or `max`.** Pass `effort` explicitly on every Agent call.
- **Selection guidelines (orchestrator = Opus 5.5; adapt one tier down for other orchestrators):**
  - **Sonnet 5.5, `effort: "high"`:** complex multi-file features or refactors, tricky debugging, non-trivial logic, anything needing careful reasoning.
  - **Sonnet 5.5, `effort: "medium"`:** standard implementation, bug fixes, tests, code search/exploration, docs lookups.
  - **Sonnet 5.5, `effort: "low"`:** small well-specified edits that still need some judgment.
  - **Haiku 5.5 (lowest effort that fits):** trivial work such as single-file mechanical edits, renames, formatting, boilerplate, running commands and summarizing output, simple lookups.
  - Architecturally critical, security-sensitive, or ambiguous decisions stay in the orchestrator; delegate only the mechanical implementation.
- **Other orchestrators:** Fable 5.1 may delegate to Opus 5.5 or lower; Sonnet 5.5 delegates only to Haiku 5.5 (trivial tasks) and handles everything else directly; Haiku 5.5 works directly. The same effort limits apply (`high` at most).
- **Sub-agent briefing:** Every sub-agent prompt must be self-contained: goal, exact files/area, constraints from this global file **AND any project-specific `AGENTS.md` (tech stack blacklists, CLI tools, architecture, i18n, branching)**, and a clear definition of done. Ask for a short report (changes made, files touched, test/lint/build results), not file dumps.
- **Parallelism & Git Worktrees:** Launch independent sub-agents in parallel in a single message. Never run two writers on the same files concurrently. Use separate `git worktree` directories when parallel implementation benefits from file-system isolation (unless prohibited for serialized assets by a project's `AGENTS.md`).
- **Verification:** The orchestrator must verify sub-agent results (diff review, tests, linter, build) before marking a task complete. If a lower-tier result is inadequate, retry once with a clearer brief, then escalate the task to the same tier as the orchestrator.
- **Opt-out (explicit user request only):** If the user explicitly asks to work without sub-agents, the orchestrator must not delegate to any lower-tier model. In that case it may either work directly or spawn sub-agents of the **same model and quality level** as itself (e.g. Opus 5.5 → Opus 5.5). This preference applies only to the request where it was stated, unless the user says otherwise.
- **Exceptions:** Skip delegation for pure conversation, single quick answers, and edits so small that briefing a sub-agent costs more than doing the work.