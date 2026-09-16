# Artifact Location Protocol

## Objective

Place planning and implementation artifacts where the host project expects them. A BMAD-aware skill must not invent a parallel document tree when the repository already has BMAD configuration or established artifact locations.

## 1. Location precedence

When deciding where to read or write an artifact, use this order:

1. **Existing artifact location** — when updating/reconciling an existing artifact, update it in place unless the user explicitly requests migration.
2. **Explicit user-specified path** — use the requested location when it does not conflict with an existing authoritative artifact.
3. **Resolved BMAD project configuration** — when the project is BMAD-enabled, respect its configured artifact roots.
4. **BMAD defaults** — only if BMAD is detected but the configured values cannot be resolved.
5. **Non-BMAD fallback** — use an existing project documentation convention; if none exists, propose a path instead of silently scattering files.

Do not move existing artifacts merely to make the repository match a preferred template.

## 2. Detect a BMAD-enabled project

Treat a repository as BMAD-enabled when the project root contains a current BMAD installation/configuration, normally `_bmad/`, especially when `_bmad/scripts/resolve_config.py`, `_bmad/config.toml`, module configuration, or existing `_bmad-output/` artifacts are present.

When BMAD is detected, first resolve or inspect project configuration before choosing output paths.

Preferred resolution when executable project tools are available:

```text
uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root}
```

Use the resolved values relevant to the installed BMAD version, including where available:

```text
output_folder
planning_artifacts
implementation_artifacts
project_knowledge
```

If the resolver cannot be executed, inspect the current BMAD configuration and existing artifact tree. Do not assume `_bmad-output` when the project config points elsewhere.

## 3. BMAD artifact classes

For a BMAD project, treat these as the default semantic destinations:

```text
{planning_artifacts}
  Product Briefs
  PRDs
  UX / Design planning artifacts
  Strategic Domain Model
  Tactical Domain Model
  Release / Scope Domain Profiles
  Architecture
  Epics / planning artifacts
  planning/readiness reviews

{implementation_artifacts}
  sprint status
  implementation stories
  implementation reviews / retrospectives / build outputs

{output_folder}
  project-context.md and other root-level BMAD outputs when the installed workflow defines them there

{project_knowledge}
  long-lived project knowledge and documentation when the installed BMAD configuration designates this location
```

The skill extends BMAD with Domain Design artifacts, but those artifacts are planning artifacts; they should remain under the configured planning-artifacts root rather than under an unrelated `docs/` tree by default.

## 4. Default Domain Design location in a BMAD project

For a new layered Domain Design when no project-specific location already exists, prefer:

```text
{planning_artifacts}/domain-design/
  strategic-domain-model.md
  tactical-domain-model.md
  release-<release>-domain-profile.md
```

Optional index/compatibility artifact:

```text
{planning_artifacts}/domain-design/domain-model.md
```

The index should point to the authoritative layered artifacts instead of duplicating their content.

### Existing BMAD project exception

If a project already keeps domain design beside architecture, for example:

```text
{planning_artifacts}/architecture/<initiative>/domain-model.md
```

or another established project-specific location, preserve that location during updates. New layered companions may be placed beside it when that causes less churn and keeps the authoritative set coherent.

Use the precedence rule: **update in place before reorganizing**.

## 5. Other planning artifacts

When creating new artifacts in a BMAD project and no existing convention is present:

```text
PRD                  -> {planning_artifacts}/PRD.md or the project's existing PRD naming convention
Domain Design        -> {planning_artifacts}/domain-design/
Architecture         -> {planning_artifacts}/architecture.md or an established architecture subdirectory
Epics                 -> {planning_artifacts}/epics.md or the project's established epics structure
Readiness review      -> {planning_artifacts}/reviews/ or beside the artifact set being reviewed
Implementation handoff-> planning or implementation location according to whether it is planning-only or attached to an implementation story; preserve the project's existing convention
```

Do not rename official/established BMAD artifacts simply to match these examples.

## 6. Brownfield / modification rule

When modifying a project that already uses BMAD Method:

1. detect BMAD and resolve configured roots;
2. inventory existing authoritative artifacts before writing;
3. identify which artifact is being updated vs newly introduced;
4. update existing files in place;
5. place genuinely new planning artifacts under the configured planning-artifacts root, normally near the related planning set;
6. update references/companions so downstream BMAD artifacts point to the new authoritative files;
7. do not create a second PRD, architecture, epics, or domain model in `docs/` merely because the skill has its own template;
8. report the resolved artifact roots and actual files changed.

## 7. Non-BMAD projects

If `_bmad/` and BMAD configuration are absent:

- follow the repository's existing documentation convention;
- preserve existing artifact locations when updating;
- if a new structure is needed, a reasonable fallback is `docs/planning/` or another user-approved project path;
- do not create `_bmad-output/` merely because this skill is BMAD-compatible.

## 8. Migration rule

Artifact relocation is a separate change from artifact-content revision.

Do not automatically migrate:

```text
docs/* -> _bmad-output/*
legacy BMAD paths -> current BMAD paths
one composite domain-model.md -> a new directory
```

unless the user asks for migration or the task explicitly includes bringing the repository into current BMAD layout. When migration is requested, update all references and report moved/deprecated paths.

## 9. Completion check

Before finishing a write workflow, verify:

- the resolved output roots were respected;
- no duplicate authoritative artifact was accidentally created;
- existing artifacts were updated in place unless migration was intended;
- new Domain Design artifacts live under the BMAD planning root for BMAD-enabled projects;
- downstream references point to the current authoritative files;
- the final response names the actual paths written.