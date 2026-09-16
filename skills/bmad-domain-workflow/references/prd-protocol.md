# PRD Protocol

## Objective

Capture product intent and externally meaningful requirements without turning the PRD into a low-level design document.

## Create

1. Establish problem, users/actors, goals and non-goals.
2. Define current scope and explicit future scope.
3. Capture critical journeys/use cases.
4. Write functional requirements with stable IDs when useful.
5. Capture product-visible business rules.
6. Capture NFRs only to the level necessary to constrain solution design.
7. Record integrations, compliance constraints, dependencies, risks and open questions.
8. Separate implementation ideas into a downstream-notes section rather than presenting them as requirements.

## Update

1. Identify the change signal.
2. Locate impacted goals, journeys, requirements and constraints.
3. Preserve unaffected requirement IDs when possible.
4. Mark superseded requirements rather than silently repurposing identifiers.
5. Identify downstream domain/architecture/story impacts.

## Validate

Check:
- requirement is testable or observable;
- scope boundary is clear;
- "how" has not displaced "what";
- no key actor/journey is missing;
- non-goals prevent obvious scope leakage;
- assumptions are visible;
- requirements do not contradict one another.

## Handoff to Domain Design

Extract:
- business capabilities
- business rules/invariants
- actors
- lifecycle concepts
- external systems
- words with overloaded meanings
- actions that cross responsibility boundaries
- temporal workflows
- likely ownership conflicts

Do not pre-decide aggregates from nouns in the PRD.
