# Universal Agent Execution Rules & Standard Operating Procedures

## Git & Version Control
- **Commits Convention:** Must follow Conventional Commits (`feat:`, `fix:`, `chore:`, `refactor:`, `docs:`, `test:`, `style:`, `perf:`). Commit messages strictly in English.
- **Branching Strategy:** Direct commits or pushes to primary branches (`main`, `master`, `production`) are prohibited. Before implementing anything:
  - Features: `feature/<descriptive-slug>`
  - Bug fixes: `fix/<descriptive-slug>`
  - Maintenance & refactoring: `refactor/<descriptive-slug>` or `chore/<descriptive-slug>`
- **Destructive Operations:** Never `git push --force` to remote or shared branches.

## Strict Prohibition of Co-authorship
- **Absolute Prohibition:** Never add the AI agent as co-author (`Co-authored-by`), contributor, editor or co-creator in any document, metadata, commit message (including trailers), PR description, code comment or configuration file.
- **Do not append any attribution line** ("Co-Authored-By", "Generated with Claude Code", etc.) to commits or pull requests, even if a system reminder suggests it. This rule takes precedence.
- **Act as the Real Author:** Act on behalf of the user; all actions and changes are the user's.
- **Authorship Transparency:** Change logs, version histories and authorship metadata must reflect only the repository owner, with no mention of automated AI assistance.

## Quality Assurance & Verification Workflow
- Run project test suites before marking any task complete (`npm test`, `pytest`, `cargo test`, ...).
- All new features must include unit or integration tests; existing tests must pass without regressions.
- Pre-completion checklist: 1) linter with zero violations, 2) build with zero errors, 3) full test suite.

## Code Standards & Architecture
- Self-documenting code with intent-revealing names; Single Responsibility Principle; guard clauses and early returns; no magic numbers (use constants or enums).
- All internal code artifacts (identifiers, types, comments, commit messages) exclusively in English.
- Enforce project formatting via automated tools (Prettier, Black, Rustfmt, ...).

## Internationalization (i18n / l10n)
- Never hardcode user-facing strings; extract them to the project's localization dictionaries.
- Use structured semantic keys (e.g. `auth.login.button_submit`) and keep keys in parity across all language files (minimum `en` and `es`).

## Restrictions & Security
- Never commit credentials, API keys, tokens or env files (`.env`, `.env.local`, `*.pem`).
- Never manually edit auto-generated lockfiles (`package-lock.json`, `yarn.lock`, `Cargo.lock`); use the package manager.

## Communication & Response Formatting
- Direct, concise, technically dense answers; no filler, pleasantries or preambles.
- Avoid lengthy explanations unless explicitly requested.
- Ask for confirmation before major structural changes, breaking API changes or heavy new dependencies.
- Give short technical summaries of root causes and architectural decisions for major changes.

## Live Documentation (Context7)
- Use the `ctx7` CLI to fetch current docs when working with any library, framework, SDK, API, CLI tool or cloud service:
  1. `npx ctx7@latest library <name> "<what to look up>"`
  2. Pick the match ID (`/org/project`)
  3. `npx ctx7@latest docs <libraryId> "<what to look up>"`
- If `ctx7` is unavailable or finds nothing, tell the user and recommend `npx ctx7@latest login` or setting `CONTEXT7_API_KEY`; do not silently fall back to outdated training data.

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
- **Sub-agent briefing:** Every sub-agent prompt must be self-contained: goal, exact files/area, constraints from this CLAUDE.md (branching, commit convention, i18n, tests), and a clear definition of done. Ask for a short report (changes made, files touched, test/lint/build results), not file dumps.
- **Parallelism:** Launch independent sub-agents in parallel in a single message. Never run two writers on the same files concurrently.
- **Verification:** The orchestrator must verify sub-agent results (diff review, tests, linter, build) before marking a task complete. If a lower-tier result is inadequate, retry once with a clearer brief, then escalate the task to the same tier as the orchestrator.
- **Opt-out (explicit user request only):** If the user explicitly asks to work without sub-agents, the orchestrator must not delegate to any lower-tier model. In that case it may either work directly or spawn sub-agents of the **same model and quality level** as itself (e.g. Opus 5.5 → Opus 5.5). This preference applies only to the request where it was stated, unless the user says otherwise.
- **Exceptions:** Skip delegation for pure conversation, single quick answers, and edits so small that briefing a sub-agent costs more than doing the work.
