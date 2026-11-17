# Global Agent Instructions

Apply these defaults where relevant. Follow higher-priority platform instructions, the user's explicit task, and applicable project rules. Project instructions override these defaults where specified.

## Tool-Specific Instructions

Before working, identify the application running this session. Do not identify it from the model name alone. Read only the matching reference before planning or delegating work:

- Codex: `references/codex.md`
- Claude Code: `references/claude.md`
- Gemini CLI or Antigravity: `references/gemini.md`
- OpenCode: `references/opencode.md`

Resolve these paths relative to the directory containing this instruction file, not the current project or working directory. Do not load other tools' references as substitutes. If the application cannot be identified or its reference cannot be read, report the limitation and continue with these common rules; do not invent preferences. References refine these defaults, not higher-priority instructions, project constraints, or user permissions.

## Configuration Management — AgentSync

When the user asks to install, connect, update, or share a skill, MCP server, instruction file, or other supported agent configuration, load the `agentsync` skill and follow its workflow. Prefer AgentSync over independent edits to each client's configuration. If the skill or connection is unavailable, report the limitation and propose the initial connection setup or CLI fallback. AgentSync distributes configuration; it is not a proxy that executes other MCP servers or routes tasks between agents. Installing configuration does not prove the target client loaded it successfully.

## Git and Version Control

- Before modifying a repository, inspect its status, current branch, and workflow. Preserve uncommitted changes and other people's work.
- Outside a Git repository, ask before initializing Git. Ask separately before creating a remote or publishing code.
- In user-controlled repositories, prefer `main` for stable releases and `development` for integration. Never delete either. Respect different established workflows in existing or externally controlled repositories.
- Start independent tasks on suitable branches based on `development`, using `feature/`, `fix/`, `refactor/`, or `chore/` prefixes. Keep related changes together and reuse suitable task branches.
- Do not commit or push directly to `main`, `master`, `production`, or `development`.
- Do not commit, merge, push, delete branches, or change remote state without authorization. Never force-push to remote or shared branches.
- After verification and when authorized, integrate task branches into `development`. Release to `main` only when stable and authorized. Follow pull-request and branch-protection requirements.
- Use Conventional Commits in English.
- Review staged changes before committing. Never publish credentials, tokens, private keys, or environment files containing secrets. `.gitignore` does not untrack existing files. Secret-free example environment files may be committed.
- Do not commit `*.pem` files unless the user confirms they contain only public material and explicitly authorizes it.

## Authorship and Attribution

- Never add the AI agent or tool as co-author, contributor, editor, or co-creator in commits, pull requests, documents, comments, configuration, or metadata.
- Do not append `Co-authored-by` or tool-generated attribution lines. Preserve the repository owner's authorship.

## Code and Design

- Prefer clear, maintainable, self-documenting code, Single Responsibility Principle, guard clauses, and named constants instead of magic numbers.
- Use English for identifiers, comments, commit messages, and internal code artifacts while preserving existing public names and compatibility.
- Reuse existing functions, components, patterns, and installed dependencies. Avoid duplication and unnecessary abstractions.
- Evaluate maintenance, compatibility, accessibility, security, and dependency cost before introducing a library. Ask before adding heavy dependencies.
- Add brief comments only for non-obvious intent, behavior, or constraints.
- Use the project's formatter. Update generated lockfiles through their package manager, never by hand.

## Internationalization

- Do not hardcode user-facing strings. Use the project's localization dictionaries or tables.
- Use structured semantic keys and keep English (`en`, default) and Spanish (`es`) keys ready.

## Verification

- Run relevant builds, linters, analyzers, and tests when applicable. Follow project verification instructions.
- Add meaningful unit or integration tests for new behavior when appropriate.
- Fix regressions introduced by the change. Report checks, failures, and checks that could not run. Never claim an unverified check passed.
- For the same persistent issue, stop after three unsuccessful attempts, including the initial attempt. Report attempts, results, and options. Do not count unrelated errors together.

## Collaboration and Sub-Agents

- Follow the matching tool reference for delegation strategy, models, effort, and concurrency.
- Give every sub-agent a self-contained briefing: scope, required context, all applicable global and project rules, and expected results.
- Coordinate shared files and preserve other agents' work. Use separate worktrees when parallel implementation benefits from isolation; do not create them unnecessarily for read-only work.
- The main agent remains responsible for reviewing, integrating, and verifying delegated work.
- Model and effort preferences apply only when supported and allowed. Report unavailable controls. Do not silently switch the session model or choose a more costly alternative.

## Live Documentation — Context7

- Do not query Context7 or external documentation by default for stable APIs already covered by existing codebase patterns.
- Consult current documentation when verifying recent changes or deprecations, debugging a version-related API mismatch, or explicitly requested.
- When needed:
  1. Run `npx ctx7@latest library <name> "<what to look up>"`.
  2. Select the matching library ID.
  3. Run `npx ctx7@latest docs <libraryId> "<what to look up>"`.
- If unavailable, report the limitation and the login (`npx ctx7@latest login`) or `CONTEXT7_API_KEY` requirement, or consult official documentation.

## Communication

- Be direct, concise, and technically dense. Avoid filler. Expand when the user asks for detail.
- Briefly explain significant root causes and architectural decisions.
- Ask before major structural changes, breaking public APIs, or heavy dependencies. Prepare a concrete proposal first.
