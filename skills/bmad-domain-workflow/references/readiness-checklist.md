# Implementation Readiness Checklist

## Artifact location / project integration
- [ ] If the repository is BMAD-enabled, BMAD configuration/output roots were resolved before writing.
- [ ] Existing authoritative artifacts were updated in place unless migration was explicitly requested.
- [ ] New planning artifacts are under the configured planning-artifacts root rather than a parallel ad-hoc tree.
- [ ] `_bmad-output` was not hardcoded when the project config points elsewhere.
- [ ] No duplicate authoritative PRD, Domain Model, Architecture, or Epics artifact was accidentally created.
- [ ] Companion/source references point to the actual current artifact paths.
- [ ] Non-BMAD projects were not given `_bmad-output/` merely because this skill is BMAD-compatible.

## PRD → Strategic Domain
- [ ] The long-lived business problem/value spine is explicit.
- [ ] Major capabilities are identified without deriving them from UI/database/service structure.
- [ ] Core / Supporting / Generic classification is explicit where useful.
- [ ] Subdomains are not silently equated with bounded contexts.
- [ ] External systems have clear semantic boundaries.
- [ ] Terminology conflicts are resolved or documented.

## Strategic Domain → Tactical Domain
- [ ] Every critical business invariant has an owner.
- [ ] Aggregate Roots have explicit business justification.
- [ ] No Aggregate exists primarily because of persistence, scheduling, dispatch, recovery, worker/process, or transaction mechanics.
- [ ] Entity identity is justified by business continuity, not merely a database key.
- [ ] Value Objects/snapshots/derived intents are not over-promoted to Entities/Aggregates.
- [ ] Repository boundaries correspond to actual Aggregate Roots.
- [ ] Domain Services contain business decisions, not multi-root I/O/transaction choreography.
- [ ] Domain Events are business facts rather than technical/runtime signals.
- [ ] Intent, execution, attempt, external outcome, interaction, and conversion are not conflated where the distinction matters.

## Domain → Release / Scope Profile
- [ ] Current-release restrictions are separated from long-lived invariants.
- [ ] Supported platform/source/action/account variants are explicit.
- [ ] Current retry/attempt/lifecycle shortcuts are marked as release choices when permanence is unproven.
- [ ] Inactive future contexts/capabilities are explicit.
- [ ] Deferred decisions are not guessed.
- [ ] The release profile does not introduce new long-lived domain semantics without a domain decision.

## Domain + Release Profile → Architecture
- [ ] Bounded contexts are respected by module/service ownership.
- [ ] Aggregate consistency requirements are technically realizable.
- [ ] Data ownership matches domain ownership.
- [ ] Cross-context interactions use explicit contracts.
- [ ] Required ACLs/adapters exist in the design.
- [ ] Consistency model matches domain invariants.
- [ ] No distributed transaction is assumed accidentally.
- [ ] Runtime permits/leases/fencing/manifests/recovery remain architecture/application mechanisms unless independently business-significant.
- [ ] Architecture does not create a second owner for a domain lifecycle.
- [ ] Release-only restrictions are implemented without being hard-coded as permanent domain truth unnecessarily.

## Architecture → Epics/Stories
- [ ] Stories name relevant architecture constraints.
- [ ] Stories distinguish business invariants from release-profile constraints and technical mechanisms.
- [ ] Integration contracts are implementable.
- [ ] Stories do not introduce new system-wide architecture silently.
- [ ] Cross-cutting NFR work has an owner.
- [ ] Story acceptance criteria do not restate technical runtime state as business semantics.

## Epics/Stories → PRD and Domain
- [ ] All in-scope requirements have implementation coverage.
- [ ] No story materially expands scope without a requirement/decision.
- [ ] Acceptance criteria are traceable to product behavior, domain invariant, release constraint, or enabling architecture.
- [ ] No story reintroduces a deprecated Aggregate, identity, event, or capability whose business justification was removed.

## Simplification / change consistency
- [ ] Superseded decisions are clearly marked.
- [ ] Downstream artifacts were reviewed after upstream changes.
- [ ] Removed requirements triggered re-evaluation of Aggregates, Entities, Repositories, and events.
- [ ] Technical concepts accidentally left in domain documents were moved to Architecture/Application Coordination.
- [ ] Release constraints accidentally left in strategic/tactical design were moved to the Release/Scope Profile.
- [ ] Deferred questions have a reason and revisit condition.

## Architecture-independence tests

Run these sanity checks where useful:

1. **Technology deletion test** — if framework/database/vendor/runtime names are removed, does the Strategic/Tactical Domain Model still make sense?
2. **Release expansion test** — if another platform/account/action is added, can the long-lived domain remain mostly stable while profile/adapters/policies change?
3. **Business language test** — can a domain expert understand the domain model without knowing permits, manifests, fencing, commits, workers, or IPC?
4. **Reliability preservation test** — after technical concepts leave the domain model, does Architecture still completely explain safe dispatch, recovery, concurrency, and persistence?
5. **Complexity justification test** — for every Aggregate/Repository, can the team state the business invariant/lifecycle that justifies it?

## Final report
Report:
1. Critical blockers
2. High-risk inconsistencies
3. Medium/low issues
4. Stable strategic decisions
5. Stable tactical decisions
6. Current release-profile constraints
7. Architecture-owned mechanisms
8. Artifact-location/configuration findings
9. Deferred decisions
10. Recommended next workflow
