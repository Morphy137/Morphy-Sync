# Gemini and Antigravity-Specific Instructions

These instructions supplement the common global instructions. Apply them only when running in Gemini CLI or Antigravity.

## Delegation Strategy

- Handle small, straightforward, or tightly coupled tasks directly.
- For substantial features or large fixes, delegate well-defined independent work when supported and useful.
- Use at most two sub-agents concurrently by default.
- Avoid nested delegation unless necessary or explicitly requested.
- Respect requests to work without sub-agents.

## Model Preferences

- Whether the main session uses Gemini 4 Carbon, Argon or hydrogen, prefer Gemini 4 hydrogen with high reasoning for delegated work when supported.
- Report unavailable models, reasoning levels, or delegation features.
- Do not assume instructions can enable unavailable capabilities.
- Do not change the main session model without authorization.
