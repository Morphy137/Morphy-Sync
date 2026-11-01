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
- **Clean Code & SOLID Principles:**
  - Write self-documenting code with highly descriptive, intent-revealing variable and function names.
  - Adhere to the Single Responsibility Principle (SRP): keep modules, functions, and classes focused and concise.
  - Use guard clauses and early returns to eliminate deeply nested conditional blocks.
  - Avoid magic numbers and hardcoded values; declare constants or enums.
- **Language Monoculture:** All internal code artifacts—including variable names, function signatures, classes, types, internal comments, and commit messages—must be exclusively in English.
- **Formatting:** Strictly enforce project formatting rules via automated tools (e.g., Prettier, Black, Rustfmt).

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
