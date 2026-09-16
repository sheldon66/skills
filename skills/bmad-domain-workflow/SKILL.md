---
name: bmad-domain-workflow
description: BMAD-compatible product planning and solution-design workflow with Domain Design as a mandatory primary input to Architecture. Use when the user asks to create, update, review, validate, or reconcile a PRD, domain model, architecture, epics/stories, implementation-readiness plan, or BMAD planning artifacts.
---

# BMAD Domain Workflow

## Purpose

Run a BMAD-compatible planning workflow in ChatGPT while making Domain Design a first-class phase between product requirements and technical architecture.

Canonical flow:

`Product Brief / Inputs -> PRD -> Domain Design -> Architecture -> Epics & Stories -> Readiness -> Implementation Handoff`

The key extension is non-negotiable:

> Existing domain design is a primary architecture input, not optional background material.

Within Domain Design, keep long-lived business semantics, release-specific product choices, and technical realization separate by default.

This skill is an original BMAD-compatible extension. It is not an official BMAD Method distribution and must not claim exact parity with the installed BMAD CLI version.

## Governing domain-design principles

Apply these principles throughout the workflow:

1. **Model the business before the solution.** Separate Problem Space from Solution Space. Do not infer the domain from tables, services, UI pages, frameworks, workers, or deployment topology.
2. **Strategic design precedes tactical design.** Establish capabilities, subdomains, Core/Supporting/Generic classification, ubiquitous language, bounded contexts, ownership, and context relationships before optimizing entities or repositories.
3. **Boundaries before objects.** A bounded context is a semantic and authority boundary; an aggregate is a consistency boundary. Neither is created merely because a noun, table, or module exists.
4. **Long-lived domain != current release.** A current MVP restriction is not automatically a permanent domain invariant. Put release-specific choices in a release/scope profile unless the business proves they are enduring rules.
5. **Domain != application coordination != infrastructure.** Business decisions and invariants belong to Domain. Use-case orchestration belongs to Application. Runtime permits, manifests, fencing tokens, worker generations, transaction recovery, browser/process coordination, queues, databases, and similar mechanisms belong to Architecture/Infrastructure unless business people genuinely use them as business concepts.
6. **Facts, intent, execution, and external outcome must remain distinguishable.** Do not collapse a business intent into its execution attempt or infer external success from local completion unless the business has reliable evidence.
7. **Model only justified identity.** Use Entity/Aggregate identity only when continuity, independent lifecycle, referenceability, or consistency ownership requires it. Do not promote transient observations, derived intents, snapshots, or technical coordination records into Aggregates without evidence.
8. **Design for known variability, not imagined variability.** Preserve stable seams for known next directions; document speculative possibilities without creating universal abstractions.
9. **The model may simplify when requirements simplify.** Remove unnecessary Aggregates, identities, repositories, events, or cross-context machinery when the business no longer needs the behavior that justified them.
10. **Architecture must conform to domain ownership, while reliability stays architectural.** Reliable dispatch, idempotency, recovery, concurrency, persistence, and fencing may be sophisticated without becoming business language.
11. **Deferred is a valid decision.** If a question does not affect current correctness, record it with a revisit trigger instead of inventing a future answer.
12. **Downstream artifacts must inherit the separation.** Epics and stories may reference release constraints and architecture mechanisms, but must not rewrite them as domain invariants.

## Default interaction contract

- Communicate with the user in Chinese unless they request another language.
- Write durable project artifacts in English by default unless the user explicitly requests another artifact language.
- Preserve content when restructuring documents; do not silently shorten or delete material.
- Prefer a minimal viable design now while protecting stable extension points for future scope.
- Do not invent requirements simply to make a model look complete.
- Ask questions only when an unresolved decision materially changes the model or architecture. Otherwise, state an assumption or mark a deferred decision and continue.
- Separate facts, user decisions, assumptions, recommendations, release constraints, architecture mechanisms, and open questions.
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
   - strategic domain model
   - tactical domain model
   - release/scope domain profile
   - legacy/composite domain design
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
   - **non-blocking**: continue with a clearly marked assumption or deferred decision.
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
- `templates/strategic-domain-model-template.md`
- `templates/tactical-domain-model-template.md`
- `templates/release-scope-profile-template.md`

Use `templates/domain-design-template.md` only as a legacy/composite index or when the user explicitly requests one combined document.

Domain design is a design contract between requirements and architecture. By default it produces **three distinct layers**.

### Layer A — Strategic Domain Model

Define long-lived business semantics:
- problem-space business spine
- business capabilities
- subdomains: Core / Supporting / Generic
- ubiquitous language
- bounded contexts
- ownership and authority
- upstream/downstream relationships
- context map and relationship patterns
- cross-context published language
- external systems requiring ACLs
- deferred strategic decisions

Strategic design must remain understandable when current frameworks, databases, vendors, and deployment choices are removed.

### Layer B — Tactical Domain Model

For each relevant bounded context, model only what is justified:
- aggregates and aggregate roots
- entities
- value objects
- business invariants
- commands
- domain events
- domain services and pure domain policies
- repositories at aggregate-root boundaries
- factories only where creation rules justify them
- integration events/contracts where context boundaries are crossed
- business state machines

For each candidate Aggregate ask:
1. What invariant requires one consistency boundary?
2. Does it have independent business identity/lifecycle?
3. Will other business concepts refer to it independently?
4. Is it merely a transient observation, derived intent, snapshot/value, query view, or technical control record?

If the last answer is yes and the others are weak, do not promote it to an Aggregate.

### Layer C — Release / Scope Domain Profile

Capture choices that constrain the current release but are not proven permanent business truths:
- active/inactive bounded contexts
- supported platform/channel/source/action variants
- fixed goals or policies
- single-account/single-tenant restrictions
- current lifecycle shortcuts such as create-and-start
- current retry/attempt limits
- fixed audience derivation rules
- intentionally inactive future capabilities
- release-only defaults and assumptions

A profile rule must not silently become a long-lived invariant. If permanence is uncertain, mark it as a release constraint or deferred decision.

### Explicit architecture boundary

The following normally belong to Architecture/Application Coordination, not Domain Design:
- runtime permits/tokens/leases
- worker generations and fencing
- action/execution materialization protocols
- manifests used only to guarantee technical completeness
- transaction commit/rollback/indeterminate semantics
- crash/restart recovery protocol
- queue, scheduler, process, browser, IPC, network, storage mechanics
- observation acknowledgement protocol
- concrete database, framework, SDK, vendor-response structures

They may be referenced from Domain Design only to state a boundary, not modeled as business Aggregates/Entities unless the business independently recognizes them.

### Event classification

Do not call every event a Domain Event. Classify when useful:
- **Domain Event** — business-significant fact inside a domain.
- **Integration Event** — stable fact/contract published across a boundary.
- **Application Coordination Event** — orchestration/progress signal used to drive a use case.
- **Technical Event** — runtime, transport, persistence, worker, or infrastructure signal.

### Domain-design quality gates

A domain design is not ready for architecture if any critical issue remains:
- key business invariant has no owner
- subdomain and bounded-context concepts are conflated without explanation
- aggregate boundary is defined as an object graph instead of a consistency boundary
- an Aggregate exists mainly to solve persistence, scheduling, recovery, dispatch, or process-control mechanics
- two bounded contexts directly share mutable internal domain objects
- release-specific restrictions are written as permanent invariants without evidence
- infrastructure/application concepts are masquerading as domain concepts
- an external model leaks directly into the core domain where semantic translation is required
- cross-aggregate strong consistency is assumed without justification
- future extensibility is implemented as speculative abstraction rather than stable domain seams
- local execution facts are mislabeled as external business outcomes without evidence
- a previously justified Aggregate/identity remains after the requirement that justified it was removed

## Phase 3 — Architecture

Load:
- `references/architecture-protocol.md`
- `templates/architecture-decision-template.md`

Architecture MUST read the current strategic model, tactical model, and release/scope profile before making structural choices.

For every major architecture decision, trace it to at least one of:
- a PRD requirement
- a strategic domain boundary
- a tactical invariant
- a release-profile constraint
- an integration constraint
- an NFR
- an explicit operational constraint

Architecture must not redefine domain ownership casually. If architecture conflicts with domain design, surface the conflict and resolve it explicitly.

Architecture should establish, as applicable:
- system decomposition aligned to bounded contexts/capabilities
- module/service boundaries
- data ownership
- transaction boundaries
- application orchestration and process managers
- integration patterns
- sync vs async communication
- eventing strategy
- external adapters / ACLs
- persistence strategy
- API/contract strategy
- consistency model
- idempotency and concurrency controls
- runtime permits/leases/fencing where needed
- reliability, dispatch, crash/restart, and recovery protocols
- security/privacy boundaries
- observability
- deployment topology
- failure/retry strategy
- technology decisions and their rationale
- evolution seams for known future directions

Prefer a modular monolith unless there is concrete evidence that distributed services are necessary. A bounded context does not automatically require a microservice.

## Phase 4 — Epics and Stories

Load `references/epics-stories-protocol.md`.

Derive work from **PRD + Strategic Domain Model + Tactical Domain Model + Release/Scope Profile + Architecture**, not PRD alone.

Each epic should represent a coherent user/business capability or enabling technical slice with clear value.

Each implementation story should include enough design context to avoid rediscovering settled decisions:
- objective / user or system value
- scope
- acceptance criteria
- relevant domain concepts
- affected bounded context(s)
- invariants that must remain true
- applicable release-profile constraints
- architecture constraints
- integration contracts
- data ownership implications
- testing notes
- explicit out-of-scope items

Avoid stories that cut across many bounded contexts merely because one UI flow spans them. Do not restate architectural reliability mechanisms as domain rules in story acceptance criteria.

## Phase 5 — Readiness

Load:
- `references/readiness-checklist.md`
- `templates/review-report-template.md`

Check consistency in both directions:

`PRD -> Strategic Domain -> Tactical Domain -> Release Profile -> Architecture -> Epics/Stories`

and

`Epics/Stories -> Architecture -> Release Profile -> Tactical Domain -> Strategic Domain -> PRD`

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
2. approved strategic boundaries and vocabulary
3. approved tactical ownership/invariants
4. applicable release-profile constraints
5. approved architecture decisions
6. story/epic to implement
7. invariants and contracts that must not be violated
8. files/modules likely affected if known
9. tests/checks expected
10. unresolved non-blocking questions
11. instruction to stop and surface conflicts instead of silently redesigning the domain

Use `templates/codex-handoff-template.md`.

## Domain-first architecture invariant

Whenever `/bmad-architecture`, architecture review, epics/stories, or implementation readiness is invoked:

1. Look for existing strategic, tactical, release-profile, or legacy domain-design artifacts.
2. If they exist, read and summarize binding decisions by layer before architecture reasoning.
3. Treat long-lived domain decisions and current release-profile constraints as different kinds of constraints.
4. If architecture requires changing a strategic/tactical decision, create a **Domain Change Proposal** first:
   - current decision and owning layer
   - conflict
   - proposed domain change
   - impact
   - alternatives
5. If architecture only requires a technical mechanism, keep it in Architecture; do not promote it into the domain model for convenience.
6. Do not silently override the domain design.

This permanently replaces the need for the user to repeat:

`Use the existing domain design as a primary architecture input.`

## Change discipline

When updating existing artifacts:

1. Identify current decisions and their owning layer.
2. Identify requested change.
3. Re-evaluate the business assumptions that originally justified affected Aggregates, identities, and events.
4. Calculate downstream impact.
5. Change the smallest coherent set of artifacts.
6. Preserve unaffected decisions.
7. Remove obsolete model complexity when its business justification no longer exists.
8. Produce a change summary:
   - changed
   - unchanged
   - moved to another layer
   - deprecated/replaced
   - deferred
   - follow-up required

For a strategic/tactical domain change, inspect release profile, architecture and stories.
For a release-profile change, inspect architecture and stories without automatically changing long-lived domain semantics.
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
- When a project is complex enough to justify separate artifacts, prefer:
  - `strategic-domain-model.md`
  - `tactical-domain-model.md`
  - `release-one-domain-profile.md` or another release/scope-specific profile name
  - architecture document(s)
- A small/simple project may combine strategic and tactical content, but must still visibly separate long-lived domain semantics from release constraints and architecture mechanisms.

## Completion behavior

At the end of a workflow:
1. summarize the decisions made;
2. list unresolved blockers separately from deferred questions;
3. name the artifact(s) that should change;
4. identify any concepts moved between Domain, Application, Release Profile, and Architecture;
5. suggest the next BMAD phase only when useful;
6. never claim a file/repository was changed unless it actually was.
