# Roadmap

This roadmap is intentionally small and maintenance-oriented. It reflects planned work, not promises or adoption claims.

## v0.1.x — hardening

- [x] Publish the first `bmad-domain-workflow` skill.
- [x] Add repository-level README, license, contribution guide, and changelog.
- [x] Add OpenAI skill UI metadata.
- [ ] Add a minimal example project with PRD → Domain Design → Architecture → Epics.
- [ ] Add regression fixtures for Create / Update / Validate / Reconcile routes.
- [ ] Verify installation and invocation in current Codex CLI/IDE workflows.
- [ ] Verify packaged upload behavior in supported ChatGPT Skills surfaces.
- [ ] Document a repeatable release checklist.

## v0.2 — evaluation and distribution

- [ ] Add an automated evaluation harness for core routing and artifact-consistency rules.
- [ ] Add checks that detect broken references and missing required skill files.
- [ ] Add representative Codex implementation-handoff examples.
- [ ] Evaluate packaging the reusable skill as an installable plugin where appropriate.
- [ ] Add contributor-oriented test instructions for behavioral changes.

## Future

- [ ] Add additional reusable skills only when they represent distinct workflows rather than variants of the same prompt.
- [ ] Add versioned migration notes when skill behavior changes materially.
- [ ] Expand compatibility testing as OpenAI Agent Skills conventions evolve.

## Maintenance principles

1. Prefer evidence and explicit project decisions over generic best practices.
2. Keep workflows inspectable and deterministic enough to review.
3. Avoid overstating compatibility, adoption, or official affiliation.
4. Favor small releases with clear changelog entries.
5. Use public issues and pull requests for meaningful maintenance work.
