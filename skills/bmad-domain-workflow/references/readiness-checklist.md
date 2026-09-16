# Implementation Readiness Checklist

## PRD → Domain Design
- [ ] Every major capability has an owner/context or an explicit reason not to.
- [ ] Critical business rules map to invariants/policies.
- [ ] External systems have clear semantic boundaries.
- [ ] Terminology conflicts are resolved or documented.

## Domain Design → Architecture
- [ ] Bounded contexts are respected by module/service ownership.
- [ ] Aggregate transaction boundaries are technically realizable.
- [ ] Data ownership matches domain ownership.
- [ ] Cross-context interactions use explicit contracts.
- [ ] Required ACLs/adapters exist in the design.
- [ ] Consistency model matches domain invariants.
- [ ] No distributed transaction is assumed accidentally.

## Architecture → Epics/Stories
- [ ] Stories name relevant architecture constraints.
- [ ] Integration contracts are implementable.
- [ ] Stories do not introduce new system-wide architecture silently.
- [ ] Cross-cutting NFR work has an owner.

## Epics/Stories → PRD
- [ ] All in-scope requirements have implementation coverage.
- [ ] No story materially expands scope without a requirement/decision.
- [ ] Acceptance criteria are traceable to product behavior or enabling constraints.

## Change consistency
- [ ] Superseded decisions are clearly marked.
- [ ] Downstream artifacts were reviewed after upstream changes.
- [ ] Deferred questions have a reason and revisit condition.

## Final report
Report:
1. Critical blockers
2. High-risk inconsistencies
3. Medium/low issues
4. Stable decisions
5. Deferred decisions
6. Recommended next workflow
