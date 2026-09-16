# Epics and Stories Protocol

## Inputs

Use the approved:
- PRD
- Strategic Domain Model
- Tactical Domain Model
- applicable Release / Scope Domain Profile
- Architecture
- UX/design artifacts where relevant

If the project still has one composite Domain Design, extract these semantic layers before deriving work.

## Epic design

An epic should:
- deliver a coherent capability/outcome;
- have a clear boundary;
- identify affected contexts;
- avoid spanning unrelated contexts for convenience;
- include enabling architecture only when necessary to deliver the capability.

Do not create an Epic merely because a Bounded Context exists. Epics are delivery/value slices, not a mirror of the domain folder tree.

## Story slicing

Prefer vertical slices that produce testable behavior while preserving domain boundaries.

A story should not require the implementer to rediscover:
- strategic context ownership
- aggregate ownership
- invariants
- current release-profile restrictions
- context boundaries
- API/event contracts
- security constraints
- major architecture patterns

## Required story context

Include:
- intent/value
- acceptance criteria
- relevant requirements IDs
- strategic domain context(s)
- tactical domain concepts
- invariants
- commands/Domain Events if settled
- applicable Release/Scope Profile constraints
- architecture decisions/constraints
- integration contracts
- persistence/data ownership constraints
- test expectations
- out of scope
- unresolved assumptions/deferred decisions, if relevant

## Layer discipline in stories

Keep these statements distinguishable:

- **Business invariant** — must hold because the domain says so.
- **Release constraint** — must hold in this release/profile, but may change later.
- **Architecture mechanism** — how the software makes the behavior safe/reliable.

Example:

```text
Business: an Execution must not claim delivery without evidence.
Release: maxAttempts = 1.
Architecture: a fenced single-use runtime permit prevents duplicate dispatch.
```

Do not rewrite the runtime permit as a domain invariant or make `maxAttempts=1` look permanent unless the domain model says so.

## Consistency checks

Reject or flag a story when:
- it writes another context's owned data directly;
- it bypasses an aggregate root to mutate internal state;
- it requires an unapproved shared model;
- acceptance criteria contradict a PRD requirement;
- it introduces an architecture pattern not approved by the architecture artifact;
- it solves speculative future scope at material current cost;
- it creates a new Aggregate/Entity/Repository inside a story without upstream domain justification;
- it reintroduces an Aggregate/identity/event removed after a scope simplification;
- it turns an application/runtime coordination mechanism into business language;
- it turns a release-specific restriction into a permanent domain rule;
- it treats a local execution fact as an external success/outcome without evidence.

## Enabling technical stories

A technical story may explicitly implement Architecture-owned behavior such as:
- runtime admission/permits
- fencing/generation control
- safe external side-effect dispatch
- transaction/recovery protocol
- projections/read models
- browser/process lifecycle

Such a story should reference the business invariant or release constraint it protects, while keeping the mechanism categorized as Architecture/Application behavior.

## Brownfield rule

When modifying an existing system, minimize file/module churn consistent with a clean design. Do not spread one capability across many modules merely to match an idealized greenfield model.

When upstream domain simplification removes an old model element, prefer retiring/deleting obsolete code and contracts rather than keeping compatibility abstractions indefinitely unless real persisted/backward-compatible data requires them.
