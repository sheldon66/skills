# Domain Design Protocol

## Objective

Create a stable conceptual model of the business that can constrain architecture without being polluted by technology choices.

## 1. Domain discovery

From PRD and stakeholder language, identify:
- capabilities
- actors
- business outcomes
- rules/invariants
- lifecycle transitions
- ownership
- external concepts
- terms that mean different things in different parts of the business

Prefer verbs and business decisions over a noun inventory.

## 2. Subdomain classification

Classify only when it improves design:
- **Core** — differentiating capability where product advantage resides.
- **Supporting** — necessary and business-specific, but not the primary differentiator.
- **Generic** — commodity capability better bought/reused when reasonable.

Classification can change over time; note uncertainty.

## 3. Bounded contexts

For every candidate context, define:
- purpose
- owned language
- owned data/concepts
- decisions it can make autonomously
- invariants it protects
- inbound/outbound contracts
- what explicitly does NOT belong

A bounded context is a semantic/ownership boundary, not automatically a deployable service.

## 4. Context map

For each relationship capture:
- upstream
- downstream
- contract owner
- integration style
- translation requirement
- failure/consistency implications

Use ACL where an external/upstream model would corrupt local semantics.

## 5. Aggregates

Define aggregates from consistency requirements:
- aggregate root
- invariant(s)
- commands
- state transitions
- entities/value objects inside the boundary
- references to other aggregates by identity
- concurrency expectations

Keep aggregates small unless strong consistency truly requires a larger boundary.

## 6. Value objects

Use Value Objects when identity is irrelevant and meaning is carried by attributes.

Expect:
- immutability
- equality by value
- validation at creation
- domain-specific behavior where useful

Do not create Value Objects mechanically for every primitive.

## 7. Events and long-running coordination

Use:
- **Domain Event** for something meaningful that already happened in a domain.
- **Integration Event** for a published cross-boundary contract.
- **Policy / Process Manager / Saga** when multiple steps/aggregates must coordinate over time.

Name events in past tense.

## 8. Services and repositories

- Domain Service: business rule that does not naturally belong to one Entity/Value Object.
- Application Service: orchestration, authorization, transactions, I/O coordination; avoid housing core business rules here.
- Repository: collection-like boundary for aggregate roots; do not expose persistence internals as the domain API.

## 9. Validation

Challenge:
- Who owns each invariant?
- Can two contexts disagree legitimately about a concept?
- Does any aggregate require a distributed transaction?
- Is a technical mechanism pretending to be a domain concept?
- Are future needs driving current abstractions without evidence?
- Is there a clear translation boundary to external platforms?
- Does each key action have a clear authority/initiator and target?

## 10. Architecture handoff

Output explicit constraints:
- context boundaries to preserve
- data ownership
- transactional boundaries
- permitted cross-boundary contracts
- consistency expectations
- domain events/integration events
- ACL requirements
- known extension seams
- deferred domain decisions

These become architecture inputs.
