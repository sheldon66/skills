# Terminology

| Term | Meaning |
|---|---|
| Problem Space | The business problems, outcomes, and capabilities that exist independently of the software solution |
| Solution Space | The software structures and mechanisms used to implement the Problem Space |
| Domain | The business/problem area being modeled |
| Subdomain | A capability/problem area within the Domain |
| Core Subdomain | Differentiating capability central to product advantage |
| Supporting Subdomain | Necessary, business-specific supporting capability |
| Generic Subdomain | Commodity capability |
| Strategic Domain Model | Long-lived business spine, subdomains, language, bounded contexts, ownership and context relationships |
| Tactical Domain Model | Aggregate, Entity, Value Object, invariant, Domain Event and business-state design inside bounded contexts |
| Release / Scope Domain Profile | Current release restrictions and active variants that constrain implementation without automatically becoming permanent domain invariants |
| Bounded Context | Boundary within which a model/language is consistent and owned |
| Ubiquitous Language | Shared domain language within a bounded context |
| Context Map | Relationships and integration semantics between bounded contexts |
| Aggregate | Business consistency and transactional boundary |
| Aggregate Root | Entity through which external changes to an Aggregate are controlled |
| Entity | Domain object with continuity/identity |
| Value Object | Immutable concept defined by value rather than identity |
| Snapshot | Immutable representation of business facts at a point/cut; not automatically an Entity/Aggregate |
| Invariant | Rule that must remain true within its consistency scope |
| Command | Intent to perform an action |
| Domain Event | Business-significant fact that already occurred inside a domain |
| Integration Event | Stable fact/event contract published across a boundary |
| Application Coordination Event | Orchestration/progress signal used by application workflow; not necessarily a business fact |
| Technical Event | Runtime, transport, persistence, worker, process, or infrastructure signal |
| Domain Service | Pure domain logic not naturally owned by one Entity/Value Object |
| Application Service | Use-case orchestration around the domain |
| Process Manager | Coordinator for a multi-step workflow; may live in Application or Domain depending on whether it owns business decisions or only orchestration |
| Repository | Domain-facing collection/persistence abstraction for Aggregate Roots |
| ACL | Anti-Corruption Layer translating an external/upstream model into local semantics |
| Policy | Business decision rule; distinguish from application/runtime scheduling/admission policy |
| Actor | Business role initiating or performing an Action; do not confuse with application user/operator unless they are the same business role |
| Target | Business role/object an Action applies to |
| Business Intent | What the business wants to happen; distinct from execution/attempt/outcome |
| Execution | Lifecycle of locally realizing business intent |
| Attempt | Concrete local operation within an Execution |
| External Outcome | Fact about what the external platform/system actually accepted/delivered/did, only when reliably observable |
| Interaction | Business response/activity that occurs after engagement; not automatically the same as an external operation result |
| Conversion | Business-defined outcome achieved; not the same as local execution completion |
| Deferred Decision | Intentionally unanswered question with a reason and revisit trigger |
| ADR | Architecture Decision Record |
