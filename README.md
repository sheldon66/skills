# Agent Skills for ChatGPT and Codex

Reusable open-source Agent Skills for structured software planning and delivery workflows.

> **Status:** early-stage and actively maintained. The repository currently ships one skill, `bmad-domain-workflow`, and is intentionally small while its workflows, compatibility, and evaluation coverage are hardened.

## Current skill

| Skill | Purpose | Version |
| --- | --- | --- |
| [BMAD Domain Workflow](./skills/bmad-domain-workflow/) | A BMAD-compatible planning workflow that makes Domain Design a first-class phase between PRD and Architecture, with traceability through Epics/Stories and implementation handoff to Codex. | 0.1.0 |

## Why this exists

AI-assisted software delivery can move quickly while still losing important context between requirements, domain decisions, architecture, implementation stories, and code.

This repository packages repeatable planning rules as Agent Skills so ChatGPT and Codex can apply the same workflow consistently instead of relying on one-off prompts.

The first skill uses this canonical flow:

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

Its core rule is:

> Existing domain design is a primary architecture input, not optional background material.

## Highlights

- Domain Design is a first-class planning artifact.
- Supports Create, Update, Validate, and Reconcile workflows.
- Preserves traceability across PRD → Domain Design → Architecture → Epics/Stories.
- Favors MVP-sized solutions while preserving known extension seams.
- Separates facts, user decisions, assumptions, recommendations, and open questions.
- Produces implementation handoffs suitable for Codex/IDE agents.
- Defaults to Chinese conversation and English durable project artifacts.
- Designed as an original BMAD-compatible extension, not an official BMAD Method distribution.

## Use with Codex

OpenAI Agent Skills are reusable workflows that can be used by Codex. For local experimentation, use Codex's `$skill-installer` and ask it to install the skill from this repository:

```text
Install the bmad-domain-workflow skill from:
https://github.com/sheldon66/skills/tree/main/skills/bmad-domain-workflow
```

Then invoke it explicitly when useful:

```text
$bmad-domain-workflow
Review this repository's PRD, domain design, architecture, and epics.
Report inconsistencies without modifying files.
```

See the skill-specific [README](./skills/bmad-domain-workflow/README.md) for more examples.

## Use with ChatGPT

On eligible ChatGPT workspaces that support Skills:

1. Open **Plugins**.
2. Open **Plugin Directory → Skills**.
3. Choose **Create → Upload from your computer**.
4. Upload a packaged copy of the `skills/bmad-domain-workflow` folder.
5. Review and install the skill.

Availability and installation can vary by ChatGPT plan, workspace, and surface.

## Maintainer workflow

This repository is maintained in public. Planned maintenance work includes:

- regression/evaluation scenarios for the core planning routes;
- compatibility checks against current Agent Skills conventions;
- reusable examples and fixtures;
- release notes and versioned skill changes;
- issue triage and review of external contributions;
- Codex-assisted PR review, documentation maintenance, and release workflows.

See [ROADMAP.md](./ROADMAP.md), [CHANGELOG.md](./CHANGELOG.md), and [CONTRIBUTING.md](./CONTRIBUTING.md).

## Repository layout

```text
skills/
└── bmad-domain-workflow/
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

## Attribution and compatibility

The current workflow is BMAD-compatible in concepts and sequencing, but it is an original extension and is not an official BMAD Method release. See the skill's [ATTRIBUTION.md](./skills/bmad-domain-workflow/ATTRIBUTION.md).

## Contributing

Issues and pull requests are welcome. Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before submitting substantial workflow changes.

## License

MIT. See [LICENSE](./LICENSE).

## References

- OpenAI — Skills and plugins: https://developers.openai.com/docs/skills-and-plugins
- OpenAI — Build skills: https://developers.openai.com/docs/build-skills
- OpenAI — Codex open source: https://developers.openai.com/docs/open-source
- BMAD Method: https://github.com/bmad-code-org/BMAD-METHOD
