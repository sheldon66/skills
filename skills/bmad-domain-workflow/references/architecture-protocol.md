# Architecture Protocol

## Preconditions

Before creating or materially changing architecture:
1. Read the PRD.
2. Read Domain Design if it exists.
3. Extract binding domain decisions.
4. Identify NFRs and operational constraints.
5. Identify unresolved domain decisions that could block architecture.

## Required traceability

For each significant architecture decision, record:
- decision
- driver(s)
- affected domain context(s)
- consequences
- alternatives considered
- status: proposed / accepted / superseded

## Domain alignment review

Check:
- module/service boundary vs bounded context
- storage ownership vs context ownership
- transaction boundary vs aggregate invariant
- message/event contract vs domain/integration events
- adapter boundary vs ACL need
- API ownership vs authority
- shared database tables that violate ownership
- orchestration that accidentally centralizes domain rules

## Architecture dimensions

Cover only those relevant to the system:
- logical decomposition
- runtime/deployment topology
- data/storage
- communication
- integration
- consistency
- concurrency/idempotency
- security/privacy
- observability
- reliability/recovery
- scalability/performance
- developer workflow
- test strategy
- migration/evolution

## Default stance

- Prefer the simplest architecture that satisfies current constraints.
- Prefer modular monolith over microservices without concrete distribution drivers.
- Prefer explicit contracts over shared internal models.
- Prefer asynchronous messaging only when temporal decoupling, fan-out, resilience, or independent ownership justifies it.
- Do not use events as a universal substitute for clear synchronous application flow.

## Conflict handling

If architecture conflicts with approved domain design, do not patch around it.

Create a Domain Change Proposal:
1. existing domain decision
2. architecture conflict
3. why architecture cannot honor it
4. proposed domain change
5. impact
6. alternatives
7. decision required
