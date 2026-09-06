---
name: bootstrap-project
description: Bootstrap an AI-ready project context by turning repository analysis into durable context, architecture, development, decision, and agent-contract documents.
disable-model-invocation: true
---

# Bootstrap Project

Create a durable operating base for developers and AI agents. Use `project-analysis.md` or `project-analysis.json` when available; otherwise perform or request a standard analysis. Existing project documentation is authoritative until a conflict is surfaced and resolved.

## Workflow

1. Read applicable `AGENTS.md`/`CLAUDE.md`, existing context, architecture, development, ADR, and policy documents. Identify stale, duplicated, and conflicting sources.
2. Build the shared domain vocabulary: objectives, actors, terms, invariants, constraints, and unresolved questions.
3. Map modules and services with responsibilities, ownership, allowed dependencies, forbidden knowledge, public seams, and change locality.
4. Document setup, commands, test/build/typecheck flows, CI, deployment, rollback, migrations, observability, and troubleshooting from evidence.
5. Write the AI operating contract: autonomous actions, confirmation-required actions, sensitive zones, required verification by change type, stop conditions, secret handling, and expected work-report format.
6. Record durable decisions as ADRs or update the repository's existing decision format. Do not invent decisions for unresolved choices.
7. Generate or update only the files the user approved. Preserve local style and minimize duplication by linking to existing sources.
8. Optionally generate `development-reference.html` and `project-analysis.json`; state that Markdown is the normative source and include provenance and verification dates.

## Default artifacts

- `CONTEXT.md`: domain glossary, objectives, invariants, constraints, and open questions.
- `ARCHITECTURE.md`: components, ownership, boundaries, dependency direction, seams, and flows.
- `DEVELOPMENT.md`: setup, commands, workflow, completion criteria, and troubleshooting.
- `AI-CONTRACT.md`: permissions, sensitive areas, stop rules, verification matrix, and reporting format.
- Existing ADR directory or `DECISIONS/`: decisions and rationale.

Do not overwrite an existing file silently. Show a diff or a proposed merge when content conflicts.

## AI contract minimum

The contract must answer:

- What may the agent read, edit, execute, or delete autonomously?
- What requires confirmation?
- Which files, data, credentials, migrations, and production actions are sensitive?
- Which checks are mandatory after each class of change?
- When must the agent stop and ask for clarification?
- How must the agent report evidence, uncertainty, tests, and remaining risks?

## Idempotence

Two runs with unchanged repository evidence must produce no substantive diff. Store source paths and `verifiedAt` metadata where useful, and distinguish changed evidence from formatting-only changes.
