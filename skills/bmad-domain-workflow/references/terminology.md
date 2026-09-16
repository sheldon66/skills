# Terminology

| Term | Meaning |
|---|---|
| Domain | The business/problem area being modeled |
| Subdomain | A capability area within the domain |
| Core Subdomain | Differentiating capability central to advantage |
| Supporting Subdomain | Necessary, business-specific supporting capability |
| Generic Subdomain | Commodity capability |
| Bounded Context | Boundary within which a model/language is consistent and owned |
| Ubiquitous Language | Shared domain language within a bounded context |
| Context Map | Relationships and integration semantics between bounded contexts |
| Aggregate | Consistency and transactional boundary |
| Aggregate Root | Entity through which external changes to an aggregate are controlled |
| Entity | Domain object with continuity/identity |
| Value Object | Immutable concept defined by value rather than identity |
| Invariant | Rule that must remain true within its consistency scope |
| Command | Intent to perform an action |
| Domain Event | Domain-significant fact that already occurred |
| Integration Event | Cross-boundary published event contract |
| Domain Service | Domain logic not naturally owned by one entity/value object |
| Application Service | Use-case orchestration around the domain |
| Repository | Domain-facing collection/persistence abstraction for aggregate roots |
| ACL | Anti-Corruption Layer translating an external/upstream model into local semantics |
| Policy / Process Manager | Coordinator reacting to events and driving multi-step domain workflow |
| ADR | Architecture Decision Record |
