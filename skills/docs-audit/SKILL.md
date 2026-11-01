---
name: docs-audit
description: Audit project documentation for accuracy, currency, structure, consistency, and usefulness against the actual repository. Use when asked to review documentation, find outdated instructions, or check whether project guides are coherent.
---

# Documentation audit

Review the documentation against the actual project and the user's requested scope. An audit is read-only unless the user also asks for changes. Respect existing project conventions and the intended audience; do not impose a standard directory structure or add documentation just to fill a template.

## Review

- Identify the intended audience and primary tasks, then inventory relevant documentation, including entry points, setup, usage, configuration, development, testing, deployment, contribution, and operational guides where these exist or are relevant. Follow repository instructions and assess representative materials within scope.
- Compare substantive claims with available evidence: source code, configuration, scripts, examples, tests, and package metadata. Check that paths, commands, configuration names, defaults, supported versions, examples, and links are consistent with the project. Distinguish verified facts from assumptions and avoid treating timestamps alone as evidence of staleness.
- Evaluate discoverability, logical organization, clear headings, navigation, and whether related information is easy to find. Identify contradictory, duplicated, missing, obsolete, or misplaced information only when it impedes a real reader or task.
- Apply documentation principles in context: accuracy, clarity, completeness for the intended task, consistency of terminology, maintainability, and accessibility. Keep conceptual explanations, step-by-step procedures, and reference material distinguishable when useful; do not require a specific framework or format.
- Check whether instructions give necessary prerequisites, safe steps, expected outcomes, and useful recovery guidance. Check examples for correctness and appropriate language or platform assumptions. Do not execute destructive commands, publish changes, or expose secrets during verification; validate links and commands only when safe and feasible.
- Verify available links and critical commands when practical. For library, framework, or platform behavior that requires current external documentation, consult current authoritative sources when available. State when links, commands, integrations, or claims could not be verified.

## Report

Prioritize actionable findings by impact. For each finding, identify the document and location, explain what is wrong or missing and its effect on the reader, cite repository evidence or a reliable source where available, and suggest a concise correction. Separate confirmed discrepancies from unverified risks. Summarize scope, checks performed, and limitations; do not claim that all documentation is current if it was not reviewed.

## When fixes are requested

Make focused corrections using existing conventions and verified evidence. Prefer updating the authoritative source and eliminating harmful contradictions over adding duplicate guides. Preserve useful context, examples, and attribution. Do not invent commands, behavior, versions, or configuration. Validate changed links, examples, or relevant project checks when feasible, and report any checks not performed.
