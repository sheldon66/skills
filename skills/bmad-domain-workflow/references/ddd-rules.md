# DDD Rules and Heuristics

## Strategic

1. Model the Problem Space before the Solution Space.
2. Strategic design precedes tactical design: capabilities/subdomains/context boundaries first, objects second.
3. Optimize boundaries around language, authority, ownership, and change cadence.
4. Do not equate organization chart, database schema, UI page, package, worker, queue, or microservice with a bounded context.
5. Shared terminology is not proof of shared semantics.
6. A context may maintain its own representation of a concept owned elsewhere.
7. Context boundaries should make authority explicit.
8. Subdomain != Bounded Context. Explain the mapping rather than assuming one-to-one.
9. Core/Supporting/Generic classification is a business differentiation decision, not a deployment classification.
10. A multi-stage value loop may contain several cooperating Core Subdomains.

## Long-lived domain vs release scope

1. A current release restriction is not automatically a permanent business invariant.
2. Put temporary platform, source, action, account-count, lifecycle, retry, or selection restrictions in a Release/Scope Profile when permanence is not established.
3. Mark undecided future behavior as Deferred rather than inventing a design.
4. Architecture and stories must consume release-profile constraints without rewriting them as long-lived domain rules.
5. When a later release broadens scope, first ask whether the strategic/tactical model already supports the variation before changing the domain model.

## Aggregate

1. Aggregate = business consistency boundary, not generic transaction wrapper.
2. One Aggregate Root is the external mutation entry point.
3. Cross-aggregate references should normally use identities rather than object references.
4. Cross-aggregate workflows favor eventual consistency unless a true business invariant demands atomicity.
5. Large aggregates require explicit justification.
6. Do not make an aggregate merely because several tables have foreign keys.
7. Do not make an aggregate merely because a process needs durable progress/recovery state.
8. Do not make an aggregate merely because a worker, queue, scheduler, or Unit of Work needs a control record.
9. Re-evaluate Aggregate justification after requirements change. Remove the Aggregate when the independent business identity/lifecycle/invariant that justified it disappears.

### Aggregate justification questions

Before introducing or retaining an Aggregate Root, answer:
- What invariant does it exclusively protect?
- Why must it be mutated through one consistency boundary?
- Does it have independent business identity/lifecycle?
- Will other business concepts refer to it independently?
- Must the business preserve it independently over time?
- Could it instead be a Value Object, immutable snapshot, transient observation, derived instruction, query view, or application/runtime coordination record?

## Entity

Use Entity when continuity/identity matters across changes.

Do not create an Entity only because persistence uses a primary key.

## Value Object

Use Value Object when the domain cares about the value, not an identity. Prefer immutable, valid-on-construction representations.

Snapshots and derived business instructions are often Value Objects when they have no independent lifecycle.

## Intent, execution, attempt, outcome

Do not collapse semantically distinct facts:

`intent != execution != attempt != external outcome != interaction != conversion`

Examples:
- A Campaign may express intent.
- An Execution may track local execution lifecycle.
- An Attempt may prove a concrete local operation began.
- A normal local return does not prove platform delivery.
- A reply is a new Interaction fact, not a rewrite of historical execution.
- A Conversion is a business outcome, not completion of local work.

Only claim the strongest outcome for which the system has reliable business evidence.

## Domain Event

An event says what happened, not what should happen. Prefer domain language over transport language.

Good:
- `LeadQualified`
- `CampaignStarted`
- `ExecutionAttempted`
- `InteractionReceived`
- `ConversionRecorded`

Avoid as Domain Events:
- `KafkaMessageSent`
- `DatabaseRowUpdated`
- `WorkerGenerationAdvanced`
- `PermitConsumed`
- `ManifestSealed` when sealing exists only for technical materialization

Classify when useful:
- Domain Event — business-significant fact.
- Integration Event — published cross-boundary contract.
- Application Coordination Event — orchestration/progress signal.
- Technical Event — runtime/infrastructure signal.

## Command

A command expresses intent:
- `QualifyLead`
- `StartCampaign`
- `PauseCampaign`
- `RecordReply`

A command can fail because invariants reject it.

Do not encode low-level transport/process operations as business commands unless they are visible business actions.

## ACL

Introduce an Anti-Corruption Layer when integrating with a system whose model:
- uses different semantics;
- changes independently;
- contains platform-specific states/codes;
- would otherwise leak vendor/platform terminology into the core domain.

External URLs, DOM structures, vendor DTOs, browser handles, cookies, SDK response codes, and provider error types should normally terminate at an adapter/ACL boundary.

## Repository

Prefer one repository interface per aggregate root where persistence abstraction is actually useful. Do not create repositories for every entity/value object.

No Aggregate Root -> normally no dedicated Repository.

## Domain vs application vs infrastructure

- **Domain**: business decisions, authority, lifecycle, invariants, business-significant facts.
- **Application**: use-case orchestration, multi-root choreography, runtime admission, coordination, transaction orchestration, recovery workflow.
- **Infrastructure**: databases, queues, IPC, APIs, browser automation, process management, vendor SDKs, concrete workers/adapters.

Technical nouns should not enter the ubiquitous language unless the business actually speaks that way.

Typical non-domain concepts:
- permit/lease/runtime slot
- worker generation/fencing token
- materialization manifest/progress
- transaction commit/rollback/indeterminate result
- crash-recovery protocol
- executor/browser lifecycle
- observation acknowledgement protocol

These may be critical to correctness and still belong to Architecture.

## Services

- Domain Service: pure business rule that does not naturally belong to one Entity/Value Object.
- Application Service: orchestrates a use case around domain objects and external capabilities.
- Infrastructure Service/Adapter: implements technical I/O.

A service that primarily loads multiple roots, coordinates commits, schedules work, issues runtime permits, dispatches workers, or reconciles crashes is usually not a Domain Service.

## Extensibility

Do not build a universal abstraction merely because more channels/actors/platforms may exist later.

Instead:
- identify stable domain role;
- isolate external variation behind ports/contracts;
- model known next variation;
- document speculative variation.

Use three buckets:
- Now
- Known next
- Speculative

## Simplification rule

DDD is not a one-way complexity ratchet.

When product decisions remove a need:
- remove unused identity linking;
- demote unnecessary Entities/Aggregates;
- delete obsolete repositories;
- remove events whose only purpose was deleted behavior;
- move technical coordination back to Architecture;
- preserve only the stable business seam that still has evidence.

## Naming

Names should answer:
- What does the business call this?
- Is that word unambiguous in this context?
- Is it a state, action, actor, role, policy, event, or thing?
- Would a product/domain expert use the same term?
- Is the word describing business meaning or merely an implementation mechanism?
