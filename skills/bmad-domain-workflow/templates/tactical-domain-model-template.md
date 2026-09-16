# Tactical Domain Model — <Project>

## 1. Purpose

State which Strategic Domain Model this document refines. Explicitly exclude release-only restrictions and technical runtime/persistence mechanisms.

## 2. Aggregate summary

| Context | Aggregate Root | Business invariant/lifecycle justification | Repository? |
|---|---|---|---|

Also list important concepts deliberately **not** modeled as Aggregate Roots and why.

## 3. Aggregate justification test

For each proposed Aggregate Root answer:

1. What business invariant requires one consistency boundary?
2. Does it have independent business identity/lifecycle?
3. Will other business concepts refer to it independently?
4. Must the business preserve it independently over time?
5. Could it instead be a Value Object, immutable snapshot, transient observation, derived instruction, query view, or application/runtime coordination record?

## 4. Tactical model by Bounded Context

### <Context Name>

#### <Aggregate Root>

**Purpose**

**Entities**

**Value Objects**

**Business invariants**

**Commands**

**Domain Events**

**Business state machine**

**References to other Aggregates**

**Repository boundary**

**What this Aggregate explicitly does not own**

## 5. Domain Services and Policies

| Service / Policy | Pure business decision | Why no single Entity/VO owns it |
|---|---|---|

Do not put transaction choreography, runtime admission, worker dispatch, persistence recovery, or crash reconciliation here.

## 6. Domain Events

| Event | Owning Context | Meaning | Trigger | Consumers |
|---|---|---|---|---|

## 7. Integration Events / Published Language

| Contract | Producer | Consumer | Stable meaning |
|---|---|---|---|

## 8. Event classification boundary

List events/signals that are intentionally **not** Domain Events:

### Application Coordination Events

### Technical Events

## 9. Intent / execution / outcome semantics

Where applicable, explicitly distinguish:

```text
business intent
!= execution lifecycle
!= attempt
!= external outcome
!= interaction
!= conversion
```

State what evidence is required before stronger outcome claims are allowed.

## 10. Repository boundaries

| Context | Aggregate Root | Repository responsibility |
|---|---|---|

No Aggregate Root normally means no dedicated Repository.

## 11. Business scenarios

Use Given / When / Then or command -> domain behavior -> state change -> Domain Event for representative scenarios.

## 12. Deferred tactical decisions

| Decision | Why deferred | Revisit trigger |
|---|---|---|

## 13. Tactical decisions

| ID | Decision | Rationale | Status |
|---|---|---|---|
| DD-T-001 | | | Proposed |

## 14. Architecture handoff

Summarize only:
- Aggregate/Repository ownership
- invariants
- business state machines
- Domain/Integration Events
- consistency expectations
- application coordination that Architecture must realize

Do not prescribe technical permits, manifests, fencing, transaction protocol, concrete database, worker, queue, browser, or IPC mechanism here.
