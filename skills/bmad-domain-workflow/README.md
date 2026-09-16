# BMAD Domain Workflow for ChatGPT

Version: **0.2.1**

This package adapts BMAD-style planning to ChatGPT and inserts a first-class, **layered Domain Design** phase:

```text
Product Brief / Inputs
        ↓
       PRD
        ↓
   Domain Design
   ├─ Strategic Domain Model
   ├─ Tactical Domain Model
   └─ Release / Scope Domain Profile
        ↓
   Architecture
        ↓
 Epics & Stories
        ↓
    Readiness
        ↓
Implementation Handoff
```

The central rules are built in:

> Existing domain design is a primary architecture input.

> Long-lived domain semantics, current release restrictions, and technical reliability mechanisms must not be silently mixed into one model.

> In an existing BMAD project, artifact paths come from the project and its BMAD configuration; the skill must not invent a parallel output tree.

You no longer need to repeat those instructions in every architecture conversation.

## What changed in 0.2.1

0.2.1 adds **BMAD-aware artifact placement**.

- Existing authoritative artifacts are updated in place by default.
- The skill detects BMAD-enabled repositories before choosing output paths.
- It respects configured `output_folder`, `planning_artifacts`, `implementation_artifacts`, and `project_knowledge` where available.
- `_bmad-output` is treated as a common default, not a hardcoded universal location.
- New layered Domain Design artifacts are planning artifacts; when no established domain location exists, they default to:

```text
{planning_artifacts}/domain-design/
  strategic-domain-model.md
  tactical-domain-model.md
  release-<release>-domain-profile.md
```

- Brownfield updates preserve existing artifact locations unless migration is explicitly requested.
- Non-BMAD repositories do not get a `_bmad-output/` directory merely because this skill is BMAD-compatible.

See `references/artifact-location-protocol.md`.

## What changed in 0.2.0

0.2.0 strengthens the Domain Design phase around lessons from real project reconciliation:

- Strategic Design precedes Tactical Design.
- Problem Space is separated from Solution Space.
- Subdomain is distinguished from Bounded Context.
- Current MVP/release restrictions move into a Release/Scope Domain Profile unless proven permanent.
- Aggregate Roots require explicit business consistency/lifecycle justification.
- Transient observations, derived intents, query views, and runtime coordination records should not become Aggregates by default.
- Domain Events are separated from Integration, Application Coordination, and Technical events.
- Intent, Execution, Attempt, external outcome, Interaction, and Conversion are kept semantically distinct.
- Runtime permits, manifests, fencing generations, commit recovery, worker/browser coordination, and similar mechanisms belong to Architecture/Application Coordination unless they genuinely exist in the business language.
- Domain complexity is allowed to shrink when product requirements shrink.

## Native ChatGPT Skill installation

If your workspace exposes Skills:

1. Open **Plugins** in the ChatGPT sidebar.
2. Open **Plugin Directory → Skills**.
3. Choose **Create → Upload from your computer**.
4. Upload the packaged skill archive.
5. Review the skill and install it.
6. Start a normal chat and either mention the skill explicitly or use a natural request such as:
   - `审查当前项目的领域设计`
   - `/bmad-domain-review`
   - `/bmad-architecture`
   - `根据 PRD、领域设计和架构重新检查 epics`

ChatGPT can automatically invoke installed skills when relevant, so explicit aliases are optional.

## Custom GPT fallback

If the Skills tab is not available:

1. Create a Custom GPT (if available on your plan/workspace).
2. Paste `INSTRUCTIONS.md` into the GPT Instructions.
3. Upload as Knowledge:
   - `SKILL.md`
   - all files under `references/`
   - all files under `templates/`
4. Save the GPT.

## Recommended usage

### Review an existing BMAD project

```text
/bmad-domain-review
Read the current BMAD project configuration and planning artifacts first.
Review the PRD, strategic/tactical domain artifacts, release profile and architecture.
Keep updates in the existing configured BMAD artifact locations.
Check whether release restrictions or runtime-reliability mechanisms leaked into the long-lived domain model.
```

### Evolve domain design

```text
/bmad-domain-design
The long-term product core is discovery → outreach → reply → conversion.
Keep current MVP scope small.
Separate the long-lived strategic model, tactical model and Release One profile.
Do not model runtime reliability mechanisms as business Aggregates.
Respect the project's existing BMAD planning-artifact location.
```

### Architecture

```text
/bmad-architecture
Create/update architecture from the approved PRD, strategic/tactical domain models and release profile.
```

The skill will automatically enforce domain design as a primary input while leaving runtime reliability mechanisms in Architecture.

### Epics

```text
/bmad-epics
Reconcile epics against PRD + Strategic Domain + Tactical Domain + Release Profile + Architecture.
```

### Codex handoff

```text
/bmad-handoff
Prepare the next implementation handoff for Codex.
```

## BMAD artifact location behavior

For an existing BMAD repository, path resolution uses this precedence:

```text
existing authoritative artifact location
  > explicit user-specified path
  > resolved BMAD configuration
  > BMAD default
  > non-BMAD fallback
```

The skill should resolve the current project's BMAD configuration where possible rather than assuming a fixed folder. BMAD commonly uses:

```text
{output_folder}/
  planning-artifacts/
  implementation-artifacts/
```

with project context and other workflow-specific outputs at configured locations. A project may configure a different output root, so `_bmad-output` is not hardcoded.

For **new Domain Design artifacts** in a BMAD project with no established domain-design location:

```text
{planning_artifacts}/domain-design/
  strategic-domain-model.md
  tactical-domain-model.md
  release-<release>-domain-profile.md
```

If the project already stores its domain model beside an architecture initiative or in another established planning directory, updates remain there unless the user asks for migration.

## Domain artifact structure

For non-trivial projects, the preferred semantic structure is:

```text
strategic-domain-model.md
  Long-lived capabilities, subdomains, ubiquitous language,
  bounded contexts, context map, authority and ACL boundaries.

tactical-domain-model.md
  Aggregates, Entities, Value Objects, invariants,
  Domain Events, business state machines and Repositories.

release-<name>-domain-profile.md
  Current release choices such as platform/action/account support,
  lifecycle shortcuts, retry limits and inactive capabilities.

architecture.md / ARCHITECTURE-SPINE.md
  Application orchestration, persistence, transaction realization,
  runtime admission, idempotency, safe dispatch, fencing and recovery.
```

Small projects may combine domain documents, but the semantic layers must remain visibly distinct.

## Design choices in this edition

- Planning-first for Chat.
- Domain Design is first-class, not an appendix to Architecture.
- Strategic, Tactical and Release-profile semantics are separated by default.
- BMAD repository configuration controls artifact roots when available.
- Brownfield artifacts are updated in place by default.
- Artifact language defaults to English; conversation defaults to Chinese.
- Create / Update / Validate / Reconcile are supported.
- MVP-first, but preserve known extension seams.
- Deferred decisions are preferred over speculative design.
- No automatic domain redesign from the architecture phase.
- Domain simplification is expected when requirements no longer justify prior model complexity.
- Bidirectional traceability at readiness:
  `PRD ↔ Strategic Domain ↔ Tactical Domain ↔ Release Profile ↔ Architecture ↔ Epics/Stories`.

## About BMAD compatibility

This package follows BMAD Method concepts and workflow ordering, but it is an original extension and does not copy or replace an installed BMAD distribution.

For exact BMAD CLI behavior, use the official BMAD Method package in your IDE and treat this Chat skill as the planning/review layer.