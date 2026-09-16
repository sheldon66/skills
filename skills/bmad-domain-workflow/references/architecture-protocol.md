# Architecture Protocol

## Preconditions

Before creating or materially changing architecture:
1. Read the PRD.
2. Read the Strategic Domain Model if it exists.
3. Read the Tactical Domain Model if it exists.
4. Read the applicable Release/Scope Domain Profile if it exists.
5. If only a legacy/composite Domain Design exists, extract those three semantic layers before reasoning.
6. Extract binding long-lived domain decisions separately from current release constraints.
7. Identify NFRs and operational constraints.
8. Identify unresolved domain decisions that could block architecture.

Do not infer domain ownership from the current code/module/database structure unless the domain artifacts are missing and the code is being used as evidence for a brownfield discovery exercise.

## Required traceability

For each significant architecture decision, record:
- decision
- driver(s)
- source layer: PRD / strategic domain / tactical domain / release profile / NFR / operational constraint
- affected domain context(s)
- consequences
- alternatives considered
- status: proposed / accepted / superseded

## Domain alignment review

Check:
- module/service boundary vs bounded context
- storage ownership vs context ownership
- transaction boundary vs aggregate invariant
- message/event contract vs Domain/Integration/Application/Technical event classification
- adapter boundary vs ACL need
- API ownership vs authority
- shared database tables that violate ownership
- orchestration that accidentally centralizes domain rules
- technical reliability state accidentally promoted to Domain Aggregate/Entity
- release-only restrictions accidentally embedded into long-lived domain modules

## Architecture-owned coordination

Architecture/Application Coordination normally owns mechanisms such as:
- Unit of Work and transaction choreography
- queues/schedulers
- runtime permits, leases, slots and admission
- worker/process generations and fencing
- technical manifests/materialization progress
- idempotency/correlation mechanisms
- safe external dispatch
- crash/restart recovery
- commit/rollback/indeterminate handling
- worker/browser/process lifecycle
- IPC/message acknowledgement
- projections/read models and rebuild strategy

These mechanisms may be essential for correctness without becoming business language.

When an architecture mechanism uses a domain term, ensure it does not become a second owner of that domain lifecycle.

## Architecture dimensions

Cover only those relevant to the system:
- logical decomposition
- runtime/deployment topology
- data/storage
- communication
- integration
- consistency
- application orchestration
- concurrency/idempotency
- safe external side-effect dispatch
- security/privacy
- observability
- reliability/recovery
- scalability/performance
- developer workflow
- test strategy
- migration/evolution

## Consistency realization

The Tactical Domain Model says **what must remain true**. Architecture decides **how the system realizes it**.

Examples:
- Domain: a Campaign start must not expose a half-started business state.
- Architecture: implement with one transaction, staged materialization, outbox, or another justified mechanism.

- Domain: an Execution cannot make an external Attempt before its business preconditions hold.
- Architecture: implement admission with a technical permit/lease/fencing protocol if needed.

Do not copy the implementation mechanism back into the domain model unless it independently has business meaning.

## Release-profile handling

Architecture must satisfy current Release/Scope Profile restrictions, but treat them as product-scope inputs rather than permanent semantic truths.

Example:
- Release Profile: `maxAttempts=1`.
- Tactical Model: Execution can represent Attempt(s) according to policy.
- Architecture: implement only one Attempt in this release without making the persistence/domain API impossible to evolve unnecessarily.

Avoid speculative extensibility: support the profile cleanly and preserve only justified seams.

## Default stance

- Prefer the simplest architecture that satisfies current constraints.
- Prefer modular monolith over microservices without concrete distribution drivers.
- Prefer explicit contracts over shared internal models.
- Prefer asynchronous messaging only when temporal decoupling, fan-out, resilience, or independent ownership justifies it.
- Do not use events as a universal substitute for clear synchronous application flow.
- Sophisticated runtime reliability is acceptable when external side effects demand it; keep that sophistication in Architecture/Application layers.

## Conflict handling

If architecture conflicts with approved domain design, do not patch around it.

First classify the conflict:

### Strategic/Tactical conflict

Create a Domain Change Proposal:
1. existing domain decision and owning layer
2. architecture conflict
3. why architecture cannot honor it
4. proposed domain change
5. impact
6. alternatives
7. decision required

### Release-profile conflict

Create a Scope/Profile Change Proposal:
1. current release constraint
2. technical/product conflict
3. proposed profile adjustment
4. impact on current scope
5. whether long-lived domain semantics remain unchanged

### Pure architecture problem

Resolve it in Architecture. Do not change the domain model merely to make persistence, scheduling, dispatch, or recovery easier.
