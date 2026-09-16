# Domain Design Protocol

## Objective

Create a stable conceptual model of the business that constrains architecture without being polluted by technology choices or by temporary release restrictions.

The default output is layered:

```text
Strategic Domain Model
  = long-lived business language, subdomains, bounded contexts, ownership

Tactical Domain Model
  = Aggregates, Entities, Value Objects, invariants, Domain Events, business state

Release / Scope Domain Profile
  = current product slice and restrictions that are not proven permanent

Architecture
  = application coordination, persistence, runtime reliability, infrastructure
```

A small project may combine the first two into one document, but it must still preserve these semantic boundaries.

## 1. Domain discovery

From PRD and stakeholder language, identify:
- problem-space business outcomes
- capabilities
- actors and business roles
- rules/invariants
- lifecycle transitions
- ownership and authority
- external concepts
- terms that mean different things in different parts of the business
- known variability vs speculative variability

Prefer verbs, decisions, outcomes, and lifecycle facts over a noun inventory.

Start by asking:

> If the current database, framework, vendor API, worker, and deployment topology disappeared, what business concepts and rules would still exist?

Those are candidates for the domain model.

## 2. Problem Space vs Solution Space

Explicitly separate:

### Problem Space

What the business is trying to accomplish and the capabilities required.

### Solution Space

How software is organized and operated to implement the business.

Do not derive Subdomains or Bounded Contexts from:
- database tables
- UI pages
- service names
- background workers
- queues
- deployment units
- framework modules

These may later implement a domain boundary, but they do not prove that the boundary exists.

## 3. Subdomain classification

Classify only when it improves design:
- **Core** — differentiating capability where product advantage resides.
- **Supporting** — necessary and business-specific, but not the primary differentiator.
- **Generic** — commodity capability better bought/reused when reasonable.

Classification can change over time; note uncertainty.

When the product's differentiation is a multi-stage value loop, Core may span multiple cooperating subdomains. Do not force the Core Domain into one technical module.

## 4. Strategic vs Release-specific decisions

Before writing an invariant, classify it:

- **Long-lived business rule** — belongs in strategic/tactical domain design.
- **Current release choice** — belongs in Release/Scope Profile.
- **Technical realization** — belongs in Architecture.
- **Unresolved future question** — belongs in Deferred Decisions.

Examples of rules that are often release-specific rather than permanent:
- one platform only
- one account only
- one supported action type
- fixed campaign goal
- create-and-start with no Draft UI
- max one Attempt
- no retry
- fixed audience derivation
- inactive reply/conversion capability

Do not elevate them to permanent invariants without business evidence.

## 5. Bounded contexts

For every candidate context, define:
- purpose
- owned language
- owned concepts/data
- decisions it can make autonomously
- invariants it protects
- inbound/outbound contracts
- what explicitly does NOT belong

A bounded context is a semantic/ownership boundary, not automatically a deployable service.

Distinguish **Subdomain** from **Bounded Context**:
- Subdomain describes Problem Space.
- Bounded Context describes a model/authority boundary in the solution model.

Document when one maps to multiple contexts or vice versa.

## 6. Context map

For each relationship capture:
- upstream
- downstream
- relationship pattern where useful
- contract owner
- published language
- integration style
- translation requirement
- failure/consistency implications

Use ACL where an external/upstream model would corrupt local semantics.

Prefer explicit relationship language such as:
- Customer / Supplier
- Published Language
- ACL
- Conformist
- Partnership
- Shared Kernel only when sharing is truly intentional

## 7. Aggregates

Define aggregates from business consistency requirements:
- aggregate root
- invariant(s)
- commands
- business state transitions
- entities/value objects inside the boundary
- references to other aggregates by identity
- concurrency expectations where they are business-relevant

Keep aggregates small unless strong consistency truly requires a larger boundary.

### Aggregate justification test

Before creating or retaining an Aggregate Root, ask:

1. What business invariant requires this consistency boundary?
2. Does the concept have independent business identity or lifecycle?
3. Do other business concepts need to refer to it independently?
4. Does the business need to preserve it independently over time?
5. Is it instead merely:
   - a transient observation,
   - a source role,
   - a derived intent/instruction,
   - an immutable value/snapshot,
   - a query/read model,
   - an orchestration record,
   - a reliability/runtime control record?

If 1–4 are weak and 5 is true, do not make it an Aggregate.

Re-run this test after requirements change. Remove obsolete Aggregates when their justification disappears.

## 8. Entities and Value Objects

Use **Entity** when continuity/identity matters across changes.

Use **Value Object** when identity is irrelevant and meaning is carried by attributes.

Value Objects should normally provide:
- immutability
- equality by value
- validation at creation
- domain-specific behavior where useful

Do not create Value Objects mechanically for every primitive.

Do not create an Entity only because persistence needs a primary key.

## 9. Intent, execution, and outcome separation

Keep these semantic layers distinct when the business needs them:

```text
Business intent
  != execution lifecycle
  != concrete attempt
  != external/platform outcome
  != user response
  != conversion
```

A local operation returning normally does not prove delivery, acceptance, reply, or conversion unless reliable business evidence establishes that fact.

This separation is especially important in automation, messaging, payments, external APIs, and other systems where the application cannot fully observe the external world.

## 10. Events and coordination

Classify events instead of treating every signal as a Domain Event:

- **Domain Event** — business-significant fact that happened in a domain.
- **Integration Event** — stable cross-boundary published contract.
- **Application Coordination Event** — orchestration/progress signal used to drive a use case.
- **Technical Event** — runtime, transport, persistence, worker, process, or infrastructure signal.

Name Domain Events in past tense.

Use Policy / Process Manager / Saga only when business or application coordination genuinely spans aggregates or time.

Do not turn a technical process manager into a Bounded Context or Aggregate merely because it persists progress.

## 11. Services and repositories

- Domain Service: business rule that does not naturally belong to one Entity/Value Object.
- Application Service: orchestration, transaction choreography, I/O coordination, runtime admission, recovery workflow.
- Repository: collection-like boundary for aggregate roots; do not expose persistence internals as the domain API.

A service that mainly loads multiple roots, coordinates a Unit of Work, retries, materializes records, dispatches work, or recovers crashes is usually Application/Architecture, not a Domain Service.

## 12. Architecture boundary test

The following normally belong outside the domain model:
- runtime permits, leases, slots, tokens
- worker/process generations and fencing
- manifests whose purpose is technical completeness/materialization
- materialization progress
- commit/rollback/indeterminate transaction states
- crash/restart reconciliation
- browser/process lifecycle
- IPC/message acknowledgement
- concrete queue/scheduler machinery
- database/storage mechanics
- vendor SDK/DTO/response structures

Keep them in Application/Architecture unless a domain expert independently recognizes them as business concepts.

## 13. Release / Scope Profile

Create a Release/Scope Profile when current scope materially restricts the long-lived model.

The profile should state:
- active/inactive contexts
- active variants of platform/source/action/policy
- current lifecycle shortcuts
- current attempt/retry policy
- current audience/selection rule
- current account/tenant limitations
- current non-goals
- explicitly deferred domain decisions

The profile constrains implementation without redefining strategic semantics.

## 14. Validation

Challenge:
- What is the long-lived business spine?
- What is Core, Supporting, Generic?
- Who owns each invariant?
- Can two contexts disagree legitimately about a concept?
- Is a Subdomain being confused with a Bounded Context?
- Does any Aggregate exist mainly because of a table, worker, transaction, or recovery mechanism?
- Does any aggregate require a distributed transaction?
- Is a technical mechanism pretending to be a domain concept?
- Is a release restriction pretending to be a permanent business invariant?
- Are local execution facts being mislabeled as external outcomes?
- Are future needs driving current abstractions without evidence?
- Is there a clear translation boundary to external platforms?
- Does each key action have a clear authority/initiator and target?
- Has a requirement removal made any Aggregate/identity/repository unnecessary?
- Are Deferred decisions explicitly marked instead of guessed?

## 15. Architecture handoff

Output explicit constraints by layer.

### Strategic constraints
- context boundaries to preserve
- ubiquitous language
- subdomain classification
- data/concept authority
- context relationships

### Tactical constraints
- aggregate ownership
- business invariants
- business state machines
- domain/integration events
- repository boundaries

### Release-profile constraints
- active variants and current scope limitations
- release-only defaults
- inactive capabilities

### Architecture-owned concerns
- application coordination
- transaction mechanisms
- consistency realization
- idempotency/concurrency
- reliability/recovery
- runtime/process control
- persistence and adapters

These become architecture inputs without merging the layers.
