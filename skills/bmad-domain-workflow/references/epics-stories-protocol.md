# Epics and Stories Protocol

## Inputs

Use the approved:
- PRD
- Domain Design
- Architecture
- UX/design artifacts where relevant

## Epic design

An epic should:
- deliver a coherent capability/outcome;
- have a clear boundary;
- identify affected contexts;
- avoid spanning unrelated contexts for convenience;
- include enabling architecture only when necessary to deliver the capability.

## Story slicing

Prefer vertical slices that produce testable behavior while preserving domain boundaries.

A story should not require the implementer to rediscover:
- aggregate ownership
- invariants
- context boundaries
- API/event contracts
- security constraints
- major architecture patterns

## Required story context

Include:
- intent/value
- acceptance criteria
- relevant requirements IDs
- domain context(s)
- domain concepts
- invariants
- commands/events if settled
- architecture decisions/constraints
- integration contracts
- persistence/data ownership constraints
- test expectations
- out of scope
- unresolved assumptions, if any

## Consistency checks

Reject or flag a story when:
- it writes another context's owned data directly;
- it bypasses an aggregate root to mutate internal state;
- it requires an unapproved shared model;
- acceptance criteria contradict a PRD requirement;
- it introduces an architecture pattern not approved by the architecture artifact;
- it solves speculative future scope at material current cost.

## Brownfield rule

When modifying an existing system, minimize file/module churn consistent with a clean design. Do not spread one capability across many modules merely to match an idealized greenfield model.
