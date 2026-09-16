# Implementation Handoff

## Target

Epic/Story:

## Approved source artifacts

- PRD:
- Strategic Domain Model:
- Tactical Domain Model:
- Release / Scope Domain Profile:
- Architecture:
- Epic/Story:

## Binding strategic domain decisions

- bounded contexts / ownership
- ubiquitous language
- context relationships

## Binding tactical domain decisions

- Aggregate ownership
- business invariants
- business state machines
- Domain / Integration Events

## Applicable release-profile constraints

List only current-release choices that constrain this implementation.

## Binding architecture decisions

Include Application/Architecture mechanisms such as transaction, concurrency, runtime admission, safe dispatch, recovery, fencing, process/browser/worker protocol where applicable.

## Invariants that must remain true

## Contracts / APIs / events

Classify event/contract type where useful:
- Domain Event
- Integration Event
- Application Coordination Event
- Technical Event

## Expected implementation scope

## Likely affected modules/files

## Test and verification expectations

## Non-blocking unresolved / deferred questions

## Layer boundary reminders

- Do not turn a release-profile restriction into a permanent domain rule.
- Do not turn runtime permits/manifests/fencing/recovery into Domain Aggregates or Ubiquitous Language unless explicitly approved as business concepts.
- Do not infer external success from local execution completion without approved evidence semantics.
- Do not add new Aggregate Roots, Entities, Repositories, or Domain Events without upstream domain justification.

## Stop conditions

Stop and surface a conflict before implementation if:
- implementation would violate a domain invariant;
- a story requires changing bounded-context ownership;
- implementation requires a new strategic/tactical domain concept not present in approved artifacts;
- architecture and domain design contradict;
- a release-profile constraint must change;
- an external contract is underspecified in a way that changes behavior;
- an old Aggregate/identity/event appears necessary only because implementation still assumes superseded domain design.

Do not silently redesign the domain during implementation.
