# Contributing

Thanks for helping improve this repository.

The goal is to keep each skill small, inspectable, reusable, and predictable across ChatGPT and Codex.

## Ways to contribute

- Report a workflow bug or ambiguous instruction.
- Propose a new validation or reconciliation rule.
- Add realistic examples or regression fixtures.
- Improve installation or compatibility documentation.
- Propose a new skill when it represents a distinct reusable workflow.

For substantial changes, open an issue first so the scope and expected behavior can be discussed before implementation.

## Pull request guidelines

A pull request should:

1. Explain the user problem being solved.
2. Identify which skill and workflow phase are affected.
3. Preserve existing behavior unless the change intentionally updates it.
4. Avoid inventing product requirements to make an example look complete.
5. Keep project-specific facts subordinate to the user's real project artifacts.
6. Update README/examples when behavior changes.
7. Update `CHANGELOG.md` for user-visible changes.

## Validation checklist

Before requesting review, exercise the affected skill against at least these scenarios where relevant:

- **Create** — the target artifact does not yet exist.
- **Update** — an artifact exists and the user requests a change.
- **Validate** — review-only; no rewrite unless requested.
- **Reconcile** — multiple artifacts disagree and need consistency analysis.
- **Downstream with missing input** — distinguish blocking from non-blocking gaps.
- **Handoff** — preserve decisions and constraints when preparing Codex/IDE implementation context.

For `bmad-domain-workflow`, also check that:

- Domain Design remains a primary input to Architecture.
- PRD, Domain Design, Architecture, and Epics/Stories remain traceable.
- The workflow does not silently redesign approved domain decisions.
- MVP scope stays small without deleting known extension seams.
- Facts, user decisions, assumptions, recommendations, and open questions remain distinguishable.

## Skill format

Each standalone skill should contain a `SKILL.md` with valid front matter, including a clear `name` and a trigger-oriented `description`.

Optional OpenAI-specific UI metadata may live in `agents/openai.yaml`.

## Versioning

Skills use semantic versioning where practical.

- Patch: documentation fixes or behavior-preserving clarifications.
- Minor: backward-compatible workflow capabilities.
- Major: materially incompatible workflow or contract changes.

Update the skill's `VERSION` file and `CHANGELOG.md` together for a release.

## Attribution

Do not describe `bmad-domain-workflow` as an official BMAD Method release. Preserve the compatibility and attribution notes in the skill package.
