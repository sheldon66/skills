# Strategic Domain Model — <Project>

## 1. Purpose and modeling boundary

State what this document owns and explicitly exclude release-specific restrictions and technical realization.

## 2. Long-lived business problem and value spine

```text
<business stage> -> <business stage> -> <business outcome>
```

Describe the Problem Space independently of frameworks, databases, vendors, workers, and deployment topology.

## 3. Business capabilities

| Capability | Business outcome | Why it matters |
|---|---|---|

## 4. Subdomain classification

| Subdomain | Type: Core / Supporting / Generic | Rationale | Confidence / notes |
|---|---|---|---|

## 5. Ubiquitous language

| Term | Bounded Context | Precise meaning | Must not be conflated with |
|---|---|---|---|

## 6. Bounded contexts

### <Context Name>

**Purpose**

**Owns**

**Decisions/authority**

**Does not own**

**Key long-lived business rules**

**Published language / contracts**

## 7. Subdomain ↔ Bounded Context mapping

| Subdomain | Bounded Context(s) | Mapping rationale |
|---|---|---|

## 8. Context map

```mermaid
flowchart LR
  A[Context A] -->|Published Language| B[Context B]
```

| Upstream | Downstream | Relationship | Contract / Published Language | ACL? |
|---|---|---|---|---|

Use relationship patterns where useful: Customer/Supplier, Published Language, ACL, Partnership, Conformist, Shared Kernel.

## 9. External systems and anti-corruption boundaries

| External system | Local context | External semantics to translate | ACL / boundary |
|---|---|---|---|

## 10. Strategic invariants / authority rules

List only long-lived business truths. Do not put current release choices here unless they are proven permanent.

## 11. Known variability and extension seams

### Now

### Known next

### Speculative — document only

## 12. Deferred strategic decisions

| Decision | Why deferred | Revisit trigger |
|---|---|---|

## 13. Strategic decisions

| ID | Decision | Rationale | Status |
|---|---|---|---|
| DD-S-001 | | | Proposed |

## 14. Constraints handed to Tactical Design / Architecture

Summarize:
- boundaries to preserve
- ownership/authority
- stable vocabulary
- context relationships
- required ACLs
- known extension seams
