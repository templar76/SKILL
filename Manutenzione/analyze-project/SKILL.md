---
name: analyze-project
description: Analyze a software repository when the user asks for an architecture map, technology-stack inventory, dependency overview, workflow map, or project health report.
disable-model-invocation: true
---

# Analyze Project

Produce a fact-grounded, standardized project report and, when requested, a development reference package. Act as a sparring partner: distinguish evidence from inference, surface unknowns, and challenge architectural assumptions without turning the report into an unsolicited rewrite plan.

## Analysis modes

Choose the smallest mode that answers the request:

- `quick`: manifests, stack, bounded tree, and obvious entry points.
- `standard` (default): quick analysis plus runtime flows, tests, CI/deployment, architecture, and critical points.
- `deep`: standard analysis plus symbol-level tracing, monorepo dependency edges, configuration surface, and prioritized risk hypotheses.

## Workflow

1. Establish scope. Identify the repository root, read applicable `AGENTS.md`/`CLAUDE.md` instructions, note whether the tree is clean or has user changes, and detect monorepo/workspace manifests.
2. Inspect the primary manifests and configuration actually present (`package.json`, lockfiles, `pyproject.toml`/`requirements.txt`, `pom.xml`, `go.mod`, `Cargo.toml`, Docker/CI files, and equivalents). Record versions, scripts, entry points, and dependency groups.
3. Build a bounded tree using `rg --files` or an equivalent fast listing. Exclude `.git`, `node_modules`, `dist`, `build`, `target`, `.venv`, caches, and coverage output unless one is relevant evidence.
4. Trace real entry points through routing, orchestration, persistence, external services, jobs, and error boundaries. Include development and delivery flows from scripts, CI, and deployment configuration.
5. Classify architecture only when supported by evidence. Label conclusions `observed`, `inferred`, or `unknown`; allow hybrid or inconsistent patterns.
6. Identify coupling and boundaries, configuration/secrets, data ownership, observability, test signals, deployment risk, security-sensitive surfaces, and maintenance hotspots. Rank findings by impact and confidence.
7. Verify commands proportionally. In `quick`, do not run project commands. In `standard` or `deep`, run read-only checks only when requested or clearly authorized; report exit status and limitations. Never install dependencies implicitly.
8. Write the report below. Include a risk/impact matrix and five next actions. If development rules are requested, read [references/development-rules.md](references/development-rules.md) and optionally use [references/development-reference.html](references/development-reference.html); never silently modify policy files.

Each step is complete only when claims have path-based evidence or are explicitly marked unknown. Never expose secret values; report variable names and locations only.

## Report format

```markdown
# Project Analysis: <name>

## Executive summary
<purpose, architecture, and three consequential observations>

## Objectives and domain
- Stated objective: <...>
- Evidence: <paths/docs>
- Open questions: <...>

## Technology stack
| Area | Technology/version | Evidence | Confidence |
|---|---|---|---|

## Repository map
<bounded tree and responsibility of significant directories>

## Dependencies and configuration
<runtime, development, infrastructure, integrations, scripts, and environment variables>

## Architecture and runtime flows
<entry points, components, boundaries, data flow, async flows, and deployment>

## Testing and delivery
<frameworks, commands, organization, CI, release path, and gaps>

## Critical points
| Priority | Finding | Evidence | Impact | Confidence |
|---|---|---|---|---|

## Risk / impact matrix
<confirmed risks and hypotheses grouped by likelihood and impact>

## Recommended next actions
1. <highest-leverage action>
2. <...>
3. <...>
4. <...>
5. <...>

## Sparring questions
<3–7 questions that challenge assumptions or expose decisions>

## Unknowns and next investigation
<items unresolved from the repository>
```

Keep the report scan-friendly while preserving evidence for every non-trivial conclusion.

## Optional machine-readable output

When requested, also produce `project-analysis.json` with stable keys: `project`, `mode`, `objectives`, `stack`, `packages`, `directoryMap`, `dependencies`, `flows`, `architecture`, `testing`, `risks`, `nextActions`, `sparringQuestions`, and `unknowns`. Every claim includes `evidence` and `confidence`; secrets never appear.
