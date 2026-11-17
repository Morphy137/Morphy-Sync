# Codex-Specific Instructions

These instructions supplement the common global instructions. Apply them only when the session is running in Codex.

## Delegation Strategy

- Handle small, straightforward, or tightly coupled tasks directly.
- For substantial features or large fixes, delegate well-defined independent work when supported and useful.
- Use at most two sub-agents concurrently by default.
- Avoid nested delegation unless necessary or explicitly requested.
- Respect requests to work without sub-agents.

## Model Preferences

- Prefer GPT-6.1 Sol medium for starting projects and acting as the main agent. Do not change the session model without authorization.
- For delegated code changes, prefer GPT-6.1 Terra.
- Use high reasoning for complex work and medium or low for simpler, bounded work when supported.
- Handle trivial work directly when delegation adds overhead.
- Do not select Astra as an automatic escalation.
- If Terra is unavailable or a different model is needed, explain the limitation and ask before choosing a more costly alternative.
