---
name: define-dev-rules
description: Define, audit, or update project development rules for humans and AI agents from repository evidence.
disable-model-invocation: true
---

# Define Development Rules

Create rules that are scoped, evidence-based, testable, and usable by both developers and AI agents. Read `project-analysis.md`, `CONTEXT.md`, `ARCHITECTURE.md`, and `DEVELOPMENT.md` when present. Distinguish `existing`, `recommended`, and `decision-needed`; never present a recommendation as an existing convention.

## Workflow

1. Identify the repository instructions, tools, scripts, history, and existing policy files. Preserve user changes and report conflicts.
2. Inspect formatter, linter, type checker, test runner, CI, dependency manager, security configuration, and deployment files. Use these as the primary evidence for current rules.
3. Define rules for style, testing, error handling, security, dependencies, commits/review, architecture boundaries, and operations only where the project has evidence or a justified proposal.
4. Define ownership and allowed dependency direction for each significant boundary. Record invariants, sensitive areas, and rules for change locality.
5. Produce the change-to-verification matrix. Cover API, domain, persistence, UI, authentication, and infrastructure when present.
6. Separate existing rules, recommended rules with adoption cost, and decisions the team must make. Ask before mutating policy files.
7. On request, write the approved rules to the requested file and generate a human-facing `development-reference.html`; keep Markdown or JSON as the normative source.

## Rule format

| Rule | Scope | Status | Rationale | Enforcement | Evidence |
|---|---|---|---|---|---|

Every rule must identify an observable enforcement mechanism: command, CI check, review criterion, schema, or documented invariant. Generic rules without scope or enforcement are incomplete.

## Change → verification matrix

| Changed area | Minimum test | Additional verification | Risk |
|---|---|---|---|

Do not claim coverage that was not verified. If a command was not run, record that explicitly.

## Output

Return:

1. Existing rules.
2. Recommended rules.
3. Decisions needed.
4. Change → verification matrix.
5. Proposed file changes, if any.

Never include secret values. Do not create or edit `AGENTS.md`, `.cursorrules`, or equivalent policy files without explicit user authorization.
