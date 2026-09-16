# Release / Scope Domain Profile — <Project / Release>

## 1. Purpose

This document constrains the current release without redefining long-lived domain semantics.

State the Strategic and Tactical Domain Model versions this profile applies to.

## 2. Release business slice

```text
<current release flow>
```

State where this release stops relative to the long-lived business spine.

## 3. Active / inactive contexts

| Bounded Context | Status | Active capability in this release |
|---|---|---|

## 4. Supported variants

| Variation axis | Current release choice | Permanent domain rule? |
|---|---|---|
| Platform | | No / Yes / Unknown |
| Source type | | |
| Action type | | |
| Account / tenant count | | |
| Campaign goal | | |
| Audience derivation | | |

## 5. Lifecycle restrictions

Document current shortcuts/limitations such as:
- create-and-start vs Draft
- scheduled start availability
- pause/resume behavior
- cancellation availability

For each, mark whether the long-term behavior is **Undecided**, not silently forbidden.

## 6. Attempt / retry policy

| Concern | Current release |
|---|---|
| Max attempts | |
| Automatic retry | |
| Manual retry/resend | |
| External verification | |

These are release constraints unless Tactical Domain Design proves they are permanent invariants.

## 7. Current audience / eligibility policy

Document:
- how the current Audience is derived
- selection/filtering availability
- exclusions/frequency/risk policies currently active or inactive

## 8. Current channel/account assumptions

Document current ChannelAccount count, capability assumptions, supported operation types, and intentionally absent capability probing/dynamic assignment.

## 9. Inactive strategic capabilities

List long-lived strategic contexts/capabilities not implemented in this release, such as Interaction, Conversation, Conversion, Risk, Billing, etc.

## 10. Release-only non-goals

## 11. Deferred domain decisions

| Decision | Current stance | Revisit trigger |
|---|---|---|

Examples:
- whether Campaign gains Draft/Scheduled lifecycle
- whether Audience becomes independently saved/reusable

## 12. Architecture implications

State what Architecture must implement for this release, without turning the mechanism into domain language.

Examples:
- serial execution
- one active runtime activity
- safe external dispatch
- recovery/fencing requirements

## 13. Profile decisions

| ID | Decision | Rationale | Status |
|---|---|---|---|
| DD-R-001 | | | Proposed |

## 14. Expansion test

Describe what should change if the next release adds another platform/account/action while preserving Strategic/Tactical semantics.
