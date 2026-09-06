# Development Rules Reference

Use this reference when the user asks to define or audit development rules. Treat it as a checklist, not as a claim that every rule applies. This Markdown file is the AI-readable source of truth; the companion HTML is for human browsing.

## Categories

- **Style:** derive naming, formatting, linting, typing, and organization from configured tools and nearby code.
- **Testing:** require tests for new behavior at the level used by the project; name the relevant command.
- **Errors:** preserve the existing error model, validate at boundaries, retain context, and avoid swallowed failures.
- **Security:** keep credentials and personal data out of source and logs; use the existing environment/configuration mechanism; validate untrusted input.
- **Dependencies:** respect the package manager and lockfile; flag issues rather than changing dependencies during analysis.
- **Commits:** use the team's convention when discoverable from history or tooling; otherwise mark it undecided.
- **Architecture:** document ownership and allowed dependency direction for each boundary; flag cycles and cross-layer reach-through.
- **Operations:** state health checks, logging, metrics, rollback, and migration expectations only when supported by deployment evidence.

## Output contract

Separate **Existing rules** (evidence from config/docs/code), **Recommended rules** (rationale and adoption cost), and **Decisions needed** (choices for the team). Rules must be testable and scoped. Do not create or edit `.cursorrules`, `AGENTS.md`, or other policy files unless explicitly requested.
