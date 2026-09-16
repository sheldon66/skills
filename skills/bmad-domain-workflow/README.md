# BMAD Domain Workflow for ChatGPT

Version: **0.1.0**

This package adapts BMAD-style planning to ChatGPT and inserts a first-class Domain Design phase:

```text
Product Brief / Inputs
        ↓
       PRD
        ↓
  Domain Design
        ↓
   Architecture
        ↓
 Epics & Stories
        ↓
    Readiness
        ↓
Implementation Handoff
```

The central rule is built in:

> Existing domain design is a primary architecture input.

You no longer need to repeat that instruction in every architecture conversation.

## Native ChatGPT Skill installation

If your workspace exposes Skills:

1. Open **Plugins** in the ChatGPT sidebar.
2. Open **Plugin Directory → Skills**.
3. Choose **Create → Upload from your computer**.
4. Upload `bmad-domain-workflow-skill-v0.1.0.zip`.
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

### Review an existing project

```text
/bmad-domain-review
Read the attached PRD, domain design, and architecture.
Review only; do not modify files.
```

### Evolve domain design

```text
/bmad-domain-design
The long-term product core is discovery → outreach → reply → conversion.
Keep current MVP scope small, but make the domain boundaries extensible.
```

### Architecture

```text
/bmad-architecture
Create/update architecture from the approved PRD and domain design.
```

The skill will automatically enforce domain design as a primary input.

### Epics

```text
/bmad-epics
Reconcile epics against PRD + domain design + architecture.
```

### Codex handoff

```text
/bmad-handoff
Prepare the next implementation handoff for Codex.
```

## Design choices in this edition

- Planning-first for Chat.
- Domain Design is first-class, not an appendix to Architecture.
- Artifact language defaults to English; conversation defaults to Chinese.
- Create / Update / Validate / Reconcile are supported.
- MVP-first, but preserve known extension seams.
- No automatic domain redesign from the architecture phase.
- Bidirectional traceability at readiness:
  `PRD ↔ Domain Design ↔ Architecture ↔ Epics/Stories`.

## About BMAD compatibility

This package follows current BMAD Method concepts and workflow ordering, but it is an original extension and does not copy or replace an installed BMAD distribution.

For exact BMAD CLI behavior, use the official BMAD Method package in your IDE and treat this Chat skill as the planning/review layer.
