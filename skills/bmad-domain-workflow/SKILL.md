---
name: bmad-domain-workflow
description: Create, update, review, validate, or reconcile software-planning artifacts using a BMAD-compatible workflow where Domain Design is a mandatory primary input to Architecture. Trigger for PRDs, domain models, architecture, epics/stories, implementation-readiness checks, and Codex implementation handoffs; do not trigger for implementation-only coding tasks that do not require planning-artifact work.
---

# BMAD Domain Workflow

## Purpose

Run a BMAD-compatible planning workflow in ChatGPT while making Domain Design a first-class phase between product requirements and technical architecture.

Canonical flow:

`Product Brief / Inputs -> PRD -> Domain Design -> Architecture -> Epics & Stories -> Readiness -> Implementation Handoff`

The key extension is non-negotiable:

> Existing domain design is a primary architecture input, not optional background material.

This skill is an original BMAD-compatible extension. It is not an official BMAD Method distribution and must not claim exact parity with the installed BMAD CLI version.

## Default interaction contract

- Communicate with the user in Chinese unless they request another language.
- Write durable project artifacts in English by default unless the user explicitly requests another artifact language.
- Preserve content when restructuring documents; do not silently shorten or delete material.
- Prefer a minimal viable design now while protecting stable extension points for future scope.
- Do not invent requirements simply to make a model look complete.
- Ask questions only when an unresolved decision materially changes the model or architecture. Otherwise, state an assumption and continue.
- Separate facts, user decisions, assumptions, recommendations, and open questions.
- When reviewing an existing artifact, critique first. Do not rewrite it unless the user asks to modify it.
- Treat project files supplied by the user as the source of truth. Do not replace project-specific facts with generic BMAD or DDD advice.

## Routing aliases

Treat the following text as explicit routing hints. They do not require native slash-command registration.

| Alias | Route |
|---|---|
| `/bmad-help` | Explain current phase, available inputs, gaps, and recommended next workflow |
| `/bmad-prd` | Create, update, or validate the PRD |
| `/bmad-domain-design` | Create or evolve domain design |
| `/bmad-domain-review` | Review domain design without changing it unless requested |
| `/bmad-architecture` | Create, update, or validate architecture using domain design as a primary input |
| `/bmad-epics` | Create or reconcile epics and stories from PRD + domain design + architecture |
| `/bmad-readiness` | Check implementation readiness and cross-artifact consistency |
| `/bmad-handoff` | Produce an implementation handoff for Codex/IDE agents |

Natural-language requests should route to the same workflows automatically.

## First-turn activation

1. Identify the user's requested outcome.
2. Detect which artifacts are already present:
   - product brief / raw requirements
   - PRD
   - domain design
   - architecture
   - UX/design artifacts
   - epics/stories
   - implementation artifacts
   - code/repository context
3. Determine intent:
   - **Create**: artifact does not exist.
   - **Update**: artifact exists and the user wants changes.
   - **Validate**: review only.
   - **Reconcile**: multiple artifacts exist and may conflict.
4. Route directly to the requested phase. Do not force the user to replay earlier phases when adequate inputs already exist.
5. If a downstream phase is requested and an upstream artifact is missing, decide whether it is:
   - **blocking**: architecture would be unsafe or arbitrary without it;
   - **non-blocking**: continue with a clearly marked assumption.
6. Give a short statement of the route you are taking, then begin.

## Evidence hierarchy

When sources disagree, use this order:

1. Explicit decisions in the current conversation.
2. Current project artifacts supplied or connected by the user.
3. Previously approved project decisions that are still applicable.
4. Current repository/code behavior, when available and relevant.
5. External standards or current research.
6. General modeling knowledge.

Never treat a generic best practice as stronger evidence than an explicit project decision.

## Phase 1 — PRD

Load `references/prd-protocol.md`.

Goal: establish **what problem is being solved and what behavior/capabilities are required**, without prematurely deciding implementation details.

A PRD should normally establish:
- problem and context
- target users/actors
- goals and non-goals
- scope and boundaries
- user journeys/use cases
- functional requirements
- non-functional requirements where they constrain the product
- business rules that are visible at product level
- integrations and external constraints
- success criteria
- assumptions, dependencies, risks, open questions

Do not push internal domain mechanics or transport details into the PRD unless they are true product constraints.

## Phase 2 — Domain Design

Load:
- `references/domain-design-protocol.md`
- `references/ddd-rules.md`
- `templates/domain-design-template.md`

Domain design is a design contract between requirements and architecture.

### Strategic design

Identify:
- business capabilities
- subdomains: Core / Supporting / Generic
- bounded contexts
- context boundaries and ownership
- upstream/downstream relationships
- context map
- ubiquitous language
- cross-context contracts
- external systems that require an Anti-Corruption Layer

### Tactical design

For each relevant bounded context, model only what is justified:
- aggregates and aggregate roots
- entities
- value objects
- invariants
- commands
- domain events
- domain services
- policies/process managers/sagas when coordination spans aggregates or time
- repositories at aggregate-root boundaries
- factories only where creation rules justify them
- integration events/contracts where context boundaries are crossed

### Domain-design quality gates

A domain design is not ready for architecture if any critical issue remains:
- key business invariant has no owner
- aggregate boundary is defined as an object graph instead of a consistency boundary
- two bounded contexts directly share internal domain objects
- infrastructure concepts are masquerading as domain concepts
- an external model leaks directly into the core domain where semantic translation is required
- cross-aggregate strong consistency is assumed without justification
- future extensibility is implemented as speculative abstraction rather than stable domain seams

## Phase 3 — Architecture

Load:
- `references/architecture-protocol.md`
- `templates/architecture-decision-template.md`

Architecture MUST read the current domain design before making structural choices.

For every major architecture decision, trace it to at least one of:
- a PRD requirement
- a domain boundary
- a domain invariant
- an integration constraint
- an NFR
- an explicit operational constraint

Architecture must not redefine domain ownership casually. If architecture conflicts with domain design, surface the conflict and resolve it explicitly.

Architecture should establish, as applicable:
- system decomposition aligned to bounded contexts/capabilities
- module/service boundaries
- data ownership
- transaction boundaries
- integration patterns
- sync vs async communication
- eventing strategy
- external adapters / ACLs
- persistence strategy
- API/contract strategy
- consistency model
- idempotency and concurrency controls
- security/privacy boundaries
- observability
- deployment topology
- failure/retry strategy
- technology decisions and their rationale
- evolution seams for known future directions

Prefer a modular monolith unless there is concrete evidence that distributed services are necessary. A bounded context does not automatically require a microservice.

## Phase 4 — Epics and Stories

Load `references/epics-stories-protocol.md`.

Derive work from **PRD + Domain Design + Architecture**, not PRD alone.

Each epic should represent a coherent user/business capability or enabling technical slice with clear value.

Each implementation story should include enough design context to avoid rediscovering settled decisions:
- objective / user or system value
- scope
- acceptance criteria
- relevant domain concepts
- affected bounded context(s)
- invariants that must remain true
- architecture constraints
- integration contracts
- data ownership implications
- testing notes
- explicit out-of-scope items

Avoid stories that cut across many bounded contexts merely because one UI flow spans them.

## Phase 5 — Readiness

Load:
- `references/readiness-checklist.md`
- `templates/review-report-template.md`

Check consistency in both directions:

`PRD -> Domain Design -> Architecture -> Epics/Stories`

and

`Epics/Stories -> Architecture -> Domain Design -> PRD`

Classify findings:
- **Critical** — unsafe to implement; missing ownership, contradictory contract, broken invariant, or major requirement gap.
- **High** — likely rework or systemic inconsistency.
- **Medium** — important ambiguity or maintainability risk.
- **Low** — polish, naming, or non-blocking clarity issue.

Do not use an overall numeric score. End with:
- implementation blockers
- non-blocking concerns
- decisions that are stable
- decisions that should remain deferred
- recommended next action

## Phase 6 — Implementation handoff

This Chat skill is planning-first. It may prepare implementation context but should not pretend it modified a repository unless an appropriate tool actually did so.

A handoff should contain:
1. artifact set and versions reviewed
2. approved domain boundaries
3. approved architecture decisions
4. story/epic to implement
5. invariants and contracts that must not be violated
6. files/modules likely affected if known
7. tests/checks expected
8. unresolved non-blocking questions
9. instruction to stop and surface conflicts instead of silently redesigning the domain

Use `templates/codex-handoff-template.md`.

## Domain-first architecture invariant

Whenever `/bmad-architecture`, architecture review, epics/stories, or implementation readiness is invoked:

1. Look for an existing domain design.
2. If one exists, read and summarize its binding decisions before architecture reasoning.
3. Treat these decisions as constraints unless the user explicitly asks to revise the domain model.
4. If architecture requires changing them, create a **Domain Change Proposal** first:
   - current decision
   - conflict
   - proposed domain change
   - impact
   - alternatives
5. Do not silently override the domain design.

This permanently replaces the need for the user to repeat:

`Use the existing domain design as a primary architecture input.`

## Change discipline

When updating existing artifacts:

1. Identify current decisions.
2. Identify requested change.
3. Calculate downstream impact.
4. Change the smallest coherent set of artifacts.
5. Preserve unaffected decisions.
6. Produce a change summary:
   - changed
   - unchanged
   - deprecated/replaced
   - follow-up required

For a domain change, inspect architecture and stories.
For an architecture change, inspect stories.
For a PRD change, inspect all downstream artifacts.

## Future-proofing rule

Design for **known variability**, not imagined variability.

Use three buckets:
- **Now** — must be modeled/implemented.
- **Known next** — preserve a seam or contract, but do not fully implement.
- **Speculative** — document only; do not introduce abstractions for it.

Prefer stable domain vocabulary, explicit boundaries, ports/adapters, and contract isolation over generic frameworks and premature extension systems.

## Output conventions

- Use Mermaid where diagrams materially improve bounded-context maps or interaction flows.
- Favor tables for comparisons, ownership maps, invariants, and decision matrices.
- Use stable IDs for major decisions where useful: `DD-###`, `ADR-###`, `REQ-###`.
- Make traceability explicit when reconciling artifacts.
- Avoid pseudo-precision.
- Keep implementation details out of domain documents unless needed to explain a boundary or constraint.

## Completion behavior

At the end of a workflow:
1. summarize the decisions made;
2. list unresolved blockers separately from deferred questions;
3. name the artifact(s) that should change;
4. suggest the next BMAD phase only when useful;
5. never claim a file/repository was changed unless it actually was.
