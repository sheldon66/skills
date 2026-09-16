# DDD Rules and Heuristics

## Strategic

1. Optimize boundaries around language, ownership and change cadence.
2. Do not equate organization chart, database schema, UI page, or microservice with a bounded context.
3. Shared terminology is not proof of shared semantics.
4. A context may maintain its own representation of a concept owned elsewhere.
5. Context boundaries should make authority explicit.

## Aggregate

1. Aggregate = consistency/transaction boundary.
2. One Aggregate Root is the external mutation entry point.
3. Cross-aggregate references should normally use identities rather than object references.
4. Cross-aggregate workflows favor eventual consistency unless a true invariant demands atomicity.
5. Large aggregates require explicit justification.
6. Do not make an aggregate merely because several tables have foreign keys.

## Entity

Use Entity when continuity/identity matters across changes.

## Value Object

Use Value Object when the domain cares about the value, not an identity. Prefer immutable, valid-on-construction representations.

## Domain Event

An event says what happened, not what should happen. Prefer domain language over transport language.

Good:
- `LeadQualified`
- `OutreachAttemptRecorded`
- `ConversationReplied`

Avoid:
- `KafkaMessageSent`
- `DatabaseRowUpdated`

## Command

A command expresses intent:
- `QualifyLead`
- `ScheduleOutreach`
- `RecordReply`

A command can fail because invariants reject it.

## ACL

Introduce an Anti-Corruption Layer when integrating with a system whose model:
- uses different semantics;
- changes independently;
- contains platform-specific states/codes;
- would otherwise leak vendor/platform terminology into the core domain.

## Repository

Prefer one repository interface per aggregate root where persistence abstraction is actually useful. Do not create repositories for every entity/value object.

## Domain vs application vs infrastructure

- **Domain**: decisions and invariants.
- **Application**: use-case orchestration.
- **Infrastructure**: databases, queues, APIs, browser automation, vendor SDKs.

Technical nouns should not enter the ubiquitous language unless the business actually speaks that way.

## Extensibility

Do not build a universal abstraction merely because more channels/actors/platforms may exist later.

Instead:
- identify stable domain role;
- isolate external variation behind ports/contracts;
- model known next variation;
- document speculative variation.

## Naming

Names should answer:
- What does the business call this?
- Is that word unambiguous in this context?
- Is it a state, action, actor, policy, event, or thing?
- Would a product/domain expert use the same term?
