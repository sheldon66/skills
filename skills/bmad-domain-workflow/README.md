# BMAD Domain Workflow for ChatGPT and Codex

Version: **0.1.0**

This package adapts BMAD-style planning to ChatGPT and Codex and inserts a first-class Domain Design phase:

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

## Codex installation

OpenAI Agent Skills can be used by Codex. For local installation, invoke `$skill-installer` and ask it to install this skill from:

```text
https://github.com/sheldon66/skills/tree/main/skills/bmad-domain-workflow
```

Codex automatically detects installed skills. If a newly installed or updated skill does not appear, restart Codex.

You can explicitly invoke the skill with:

```text
$bmad-domain-workflow
Review the current PRD, domain design, architecture, and epics.
Review only; do not modify files.
```

Natural-language requests that match the skill description may also trigger it when implicit invocation is enabled.

## ChatGPT Skill installation

If your eligible ChatGPT workspace exposes Skills:

1. Open **Plugins** in the ChatGPT sidebar.
2. Open **Plugin Directory → Skills**.
3. Choose **Create → Upload from your computer**.
4. Upload a packaged copy of this skill folder.
5. Review the skill and install it.
6. Start a normal chat and either mention the skill explicitly or use a natural request such as:
   - `审查当前项目的领域设计`
   - `/bmad-domain-review`
   - `/bmad-architecture`
   - `根据 PRD、领域设计和架构重新检查 epics`

ChatGPT can automatically invoke installed skills when relevant, so explicit aliases are optional.

Availability, installation, and syncing can vary by plan, workspace, and product surface.

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

## Routing aliases

These aliases are routing hints inside the skill. In Codex, the skill itself can also be explicitly selected as `$bmad-domain-workflow`.

| Alias | Route |
| --- | --- |
| `/bmad-help` | Explain the current phase, available inputs, gaps, and recommended next workflow |
| `/bmad-prd` | Create, update, or validate the PRD |
| `/bmad-domain-design` | Create or evolve domain design |
| `/bmad-domain-review` | Review domain design without changing it unless requested |
| `/bmad-architecture` | Create, update, or validate architecture using domain design as a primary input |
| `/bmad-epics` | Create or reconcile epics/stories from PRD + domain design + architecture |
| `/bmad-readiness` | Check implementation readiness and cross-artifact consistency |
| `/bmad-handoff` | Produce an implementation handoff for Codex/IDE agents |

## Design choices in this edition

- Planning-first for ChatGPT and Codex.
- Domain Design is first-class, not an appendix to Architecture.
- Artifact language defaults to English; conversation defaults to Chinese.
- Create / Update / Validate / Reconcile are supported.
- MVP-first, but preserve known extension seams.
- No automatic domain redesign from the architecture phase.
- Bidirectional traceability at readiness:
  `PRD ↔ Domain Design ↔ Architecture ↔ Epics/Stories`.
- OpenAI-specific UI metadata lives in `agents/openai.yaml`.

## Package layout

```text
bmad-domain-workflow/
├── SKILL.md
├── README.md
├── INSTRUCTIONS.md
├── ATTRIBUTION.md
├── VERSION
├── agents/
│   └── openai.yaml
├── references/
└── templates/
```

## About BMAD compatibility

This package follows current BMAD Method concepts and workflow ordering, but it is an original extension and does not copy or replace an installed BMAD distribution.

For exact BMAD CLI behavior, use the official BMAD Method package in your IDE and treat this skill as the planning/review layer.

## References

- OpenAI — Skills and plugins: https://developers.openai.com/docs/skills-and-plugins
- OpenAI — Build skills: https://developers.openai.com/docs/build-skills
- OpenAI Help — Skills in ChatGPT: https://help.openai.com/en/articles/20001066-skills-in-chatgpt
- BMAD Method repository: https://github.com/bmad-code-org/BMAD-METHOD
