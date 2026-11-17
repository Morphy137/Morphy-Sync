# OpenCode-Specific Instructions

These instructions supplement the common global instructions. Apply them only when the session is running in OpenCode.

## Delegation Strategy

- Handle small, straightforward, or tightly coupled tasks directly.
- Delegate independent work only when the current client supports it and the task benefits from it.
- Respect requests to work without sub-agents and all client permission boundaries.

## Models and Capabilities

- No OpenCode-specific model preference has been configured. Preserve the user's selected model and provider.
- Do not inherit Codex, Claude Code, or Gemini model preferences merely because OpenCode uses one of their providers.
- Report unavailable tools, MCP connections, models, or delegation features. Do not invent capabilities or silently change configuration.

## Existing OpenCode Safety and Verification Preferences

- Never bypass hooks, rewrite shared history, or run destructive cleanup commands. Ask before deployment or publication.
- Make the smallest coherent change. Do not add unrelated cleanup, dependencies, features, or migrations.
- Validate proportionately to risk. Documentation, copy, styling, and similarly low-risk changes may use focused checks instead of the full suite; state what was skipped and why.
- Distinguish inspected facts, executed checks, inferences, and unverified assumptions. List the relevant validation commands and outcomes before claiming completion.
- Treat a detected secret as a hard stop for staging, committing, pushing, or publishing. Never expose sensitive personal data or private certificates.

## Current Documentation — OpenCode Override

The existing OpenCode preference below overrides the common on-demand documentation default for this client only.

- Use the current-documentation capability for library, framework, SDK, API, CLI, and cloud-service details, including familiar technologies. Load the relevant documentation skill when available.
- Use the `ctx7` CLI for API syntax, configuration, version migration, platform or library porting, library-specific debugging, setup, and CLI usage. Prefer current primary sources over remembered APIs and report verification failures.
- Do not query documentation for general programming concepts or code-only refactoring, debugging, or reviews that do not depend on a library or platform API.
- First run `npx ctx7@latest library <name> "<what to look up>"` unless the user supplies a valid library ID. Use official library names and select the matching ID by relevance, source reputation, and available documentation.
- Then run `npx ctx7@latest docs <libraryId> "<what to look up>"`. Keep each query specific; separate independent concepts. Do not run more than three commands per question.
- For version-specific documentation, use the versioned library ID returned by the library lookup.
- Never include credentials or other sensitive information in documentation queries.
- If quota or authentication blocks the command, report the limitation and suggest `npx ctx7@latest login` or `CONTEXT7_API_KEY`. Do not silently replace current-documentation verification with remembered details.
