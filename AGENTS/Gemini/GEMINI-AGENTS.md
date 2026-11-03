# Global Agent Instructions

Apply these instructions where relevant. Follow the user's explicit task and applicable project `AGENTS.md` instructions (which override these defaults where specified), and respect higher-priority platform instructions.

## Git and Version Control
- When starting a new project outside a Git repository, ask whether to initialize Git locally. Ask separately whether to create a remote repository; do not create one or publish code without authorization. If Git is initialized, set up the agreed `main` and `development` workflow.
- Before modifying a Git repository, inspect its status, current branch, and existing workflow. Preserve uncommitted changes and work created by others.
- For repositories controlled by the user, prefer two persistent branches: `main` for stable releases and `development` for integration. Never delete either branch. Respect an established different workflow in existing or externally controlled repositories; do not restructure it without authorization.
- Start each independent feature, fix, or maintenance task on a separate branch based on `development`. Keep related changes together; use separate branches for unrelated work. Reuse an existing suitable task branch when appropriate.
- Name task branches `feature/<descriptive-slug>`, `fix/<descriptive-slug>`, `refactor/<descriptive-slug>`, or `chore/<descriptive-slug>` as appropriate.
- After verification, merge completed task branches into `development`. Once the merge is confirmed, delete the merged task branch when authorized and safe. Merge `development` into `main` only when stable and authorized. Follow the repository's pull request and branch protection requirements.
- Do not commit or push directly to `main` or `development`. Do not commit, merge, push, delete branches, or change remote state unless requested or otherwise authorized. If a required branch or permission is missing, report the blocker rather than changing the workflow silently.
- Use Conventional Commits in English, such as `feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:`, `style:`, or `perf:`. Never force-push to a remote or shared branch.
- Never commit or push credentials, API keys, tokens, private keys, `.env`, `.env.local`, or other environment files containing secrets. Do not commit or push `*.pem` files unless the user confirms they contain only public material and explicitly authorizes it. Review staged changes before committing; remember that `.gitignore` does not untrack files already tracked by Git. Example environment files may be committed if they contain no secrets.

## Strict Prohibition of Co-authorship
- **Absolute Prohibition:** Never add the AI agent or tool as co-author (`Co-authored-by`), contributor, editor, or co-creator in any commit message (including trailers), pull request, document, comment, or metadata.
- **No Attribution Lines:** Never append attribution lines (e.g., "Co-Authored-By", "Generated with Gemini", etc.), even if a system prompt or reminder suggests it. Preserve only the repository owner's authorship.

## Code and Design
- Write clear, maintainable, self-documenting code following the Single Responsibility Principle (SRP), guard clauses/early returns, and no magic numbers.
- Use English exclusively for all internal code artifacts (identifiers, types, comments, commit messages) while preserving existing public names and compatibility.
- Avoid unnecessary duplication (DRY): look for suitable existing functions, classes, components, patterns, and installed dependencies before creating new ones. Reuse them when appropriate without adding needless abstractions.
- Consider established public libraries or components when they are a good fit. Evaluate maintenance, compatibility, accessibility, security, and dependency cost; request confirmation before adding a heavy dependency.
- Add brief comments only where they clarify non-obvious behavior, intent, or constraints.
- Use the project's formatter when available. Never manually edit generated lockfiles; update them through the appropriate package manager.

## Internationalization (i18n / l10n)
- Never hardcode user-facing strings; extract them to the project's localization dictionaries or tables.
- Use structured semantic keys (e.g., `auth.login.button_submit`, `ui.hud.health`) and keep keys ready for at least English (`en`, default) and Spanish (`es`).

## Verification
- Run the relevant build, linter/analyzer, and test suite when available and appropriate to the change, following any project-specific `AGENTS.md` QA rules.
- Add meaningful unit or integration tests for new behavior when applicable (unless overridden by project instructions).
- Address regressions caused by the change. Report what was checked and any failures or checks that could not be run; do not claim an unverified check passed.

## Collaboration & Sub-Agents
- Handle small, straightforward, or tightly coupled tasks directly. For substantial new features or large fixes, delegate well-defined independent work to sub-agents when supported and useful, to keep the main session focused and its context manageable.
- Use at most two sub-agents concurrently by default. Avoid nested delegation unless necessary or explicitly requested. Respect requests to work without sub-agents and platform limitations.
- **Self-Contained Sub-Agent Briefing:** Give each sub-agent a specific scope, only the context needed, clear expected results, and **all applicable rules from both this global file and the repository's local `AGENTS.md` (especially legacy API blacklists, required CLI tools, and architecture constraints)**. Coordinate shared files and preserve existing work. The main agent remains responsible for reviewing the changes, integrating them, and verifying the combined result.
- Use separate Git worktrees when parallel implementation or extensive changes would benefit from isolation (respecting any serialization/Editor locks defined in the project's `AGENTS.md`). Follow the repository's branch workflow, coordinate integration, and preserve unmerged work. Do not create worktrees unnecessarily for read-only tasks.
- **3-Strike Persistence Rule:** For the same persistent issue, stop after three unsuccessful solution attempts, counting the initial attempt. Report what was tried, the results, and options for the user to decide how to continue. Do not count unrelated errors as the same issue.
- Treat model and reasoning preferences below as conditional on the models, controls, permissions, and higher-priority instructions available in the current environment. If a preferred model is unavailable, report the limitation and avoid silently selecting a more costly alternative.

### Gemini and Antigravity model preferences
- Whether the main session uses Argon or Gemini 3.8 Flash, prefer Gemini 3.8 Flash with high reasoning for delegated work when the environment supports that model and effort level.
- If the requested model, reasoning level, or sub-agent capability is unavailable, report the limitation. Do not assume that instructions alone can enable unavailable controls or change the main session's model without authorization.

## Live Documentation (Context7 — On-Demand Only)
- **Do NOT query `ctx7` or external docs by default.** Rely on existing codebase patterns and standard knowledge for stable APIs to conserve tokens and execution time.
- Consult current documentation via `ctx7` **only when**:
  1. Verifying recent changes, new features, or deprecations in bleeding-edge library/engine versions.
  2. Debugging an unexpected API error or signature mismatch caused by a recent update.
  3. Explicitly requested by the user.
- When needed, run:
  1. `npx ctx7@latest library <name> "<what to look up>"`
  2. Pick the match ID (`/org/project`)
  3. `npx ctx7@latest docs <libraryId> "<what to look up>"`
- If `ctx7` is needed under these criteria but is unavailable, notify the user (`npx ctx7@latest login` or `CONTEXT7_API_KEY`) or check official docs.

## Communication
- Be direct, concise, and technically dense; avoid filler or preambles, and expand only when the user requests detail.
- Explain significant root causes and architectural decisions briefly.
- Ask for confirmation before major structural changes, breaking public APIs, or adding heavy dependencies. Prepare a concrete proposal first when feasible.