# Rearctor Expansion - v0 Implementation Plan

## Status

**Concept / Research.** This plan describes research that has not started. No phase is
complete, in progress, or scheduled.

There are **no dates** in this document. Phase ordering is a dependency order, not a
timeline. **No destination network is named in any phase.** No bridging exists, no
messaging integration exists, no model is selected, and no prototype exists.

This is a research plan. Several phases may conclude that the thing being researched
should not be built.

Companion document: [v0 design specification](design-spec.md).

## Phase A - Trust model comparison

**Objective.** Determine whether any representation model has an acceptable failure mode,
and whether a mechanism-guaranteed supply invariant is worth having.

**Inputs.**
- [research/trust-models.md](../../research/trust-models.md)
- [rfcs/0001-canonical-representation.md](../../rfcs/0001-canonical-representation.md)
- Issue #1

**Deliverables.**
- A per-model failure analysis extending the existing comparison: what breaks first, who
  is harmed, and whether recovery is possible.
- An assessment of the central observation: every model preserving a global supply
  invariant does so by mechanism, and every mechanism failure is unrecoverable.
- A decision on whether a mechanism-guaranteed invariant may ever be presented alongside
  Arc's construction-guaranteed one.

**Blockers.** Issue #1 open. No model is selected and none should be until this phase
concludes.

**Exit criteria.**
- Each model has a documented worst-case failure and a recoverability assessment.
- The construction-versus-mechanism distinction is either adopted as a binding
  presentation rule or explicitly rejected with reasoning.
- A defensible conclusion is reached - including, legitimately, that no model is
  acceptable.

## Phase B - Canonical representation specification

**Objective.** Specify what makes one instance authoritative, and how identity is
resolved across ecosystems.

**Inputs.** Phase A output; the canonical representation RFC.

**Deliverables.**
- A specification of origin-canonical framing, or a reasoned rejection of it.
- An identity resolution mechanism: origin-signed registry, deterministic derivation, or
  message-attested linkage.
- A decision on whether resolution must be verifiable destination-side without reading
  Arc.

**Blockers.** Phase A incomplete. Identity resolution depends on which trust model, if
any, survives.

**Exit criteria.**
- "Which instance is the asset?" has an unambiguous answer, or the ambiguity is
  documented as unresolvable.
- Metadata convention alone is excluded, since it is unauthenticated and is the
  impersonation vector.
- Second-class status of representations is either accepted with its consequences, or
  shown to make expansion pointless.

## Phase C - Supply accounting model

**Objective.** Determine which supply accounting position is defensible and what each
costs.

**Inputs.** Phases A-B; Issue #2.

**Deliverables.**
- An analysis of positions A, B and C from the design spec.
- A treatment of how locked or burned supply interacts with the permanent Arc position,
  which cannot be a source of funds.
- A decision on whether "fixed supply" remains sayable about the asset as a whole.

**Blockers.** Phases A-B incomplete. Issue #2 open.

**Exit criteria.**
- Each position has a stated checkability requirement and a stated failure consequence.
- No path exists by which expansion draws on the permanent migrated Arc position.
- If a global invariant is claimed at all, `guaranteed_by` is recorded as `mechanism` and
  never as `construction`.

## Phase D - Messaging abstraction research

**Objective.** Understand what messaging assumptions each surviving model requires,
without selecting a provider.

**Inputs.** Phases A-C.

**Deliverables.**
- An enumeration of required assumptions: unforgeability, delivery or detectable
  absence, replay prevention, security model comprehensibility.
- An analysis of the halt case, where a frozen representation is a safe failure rather
  than a solvency failure.
- A provider-agnostic abstraction describing what a messaging layer must provide.

**Blockers.** Phases A-C incomplete.

**Exit criteria.**
- Every assumption is stated explicitly; none is absorbed silently into a design.
- The abstraction names no provider and no network.
- Safe failures and unsafe failures are distinguished, with the difference reflected in
  the abstraction.

## Phase E - Liquidity fragmentation simulation design

**Objective.** Design - not run - a study that could answer whether remote liquidity is
additional or diverted.

**Inputs.** Phases A-D; [research/liquidity-fragmentation.md](../../research/liquidity-fragmentation.md).

**Deliverables.**
- A simulation design covering depth-weighted price impact per venue, cross-venue spread,
  arbitrage share of remote volume, and origin volume before and after.
- An explicit statement of what the simulation cannot answer, given that it models
  markets that do not exist.
- A definition of what result would count as evidence against expansion.

**Blockers.** Phases A-D incomplete.

**Exit criteria.**
- The design specifies quantities measurable in principle, not merely nameable.
- A falsifiable negative outcome is defined in advance - a study that cannot conclude
  "do not do this" is not a study.
- The methodological problem is documented: the decision precedes the data.

**Explicitly not a deliverable:** simulation results. This phase designs a study; it does
not run one, and no results exist.

## Phase F - Prototype eligibility review

**Objective.** Review whether the accumulated research supports specifying an eligibility
mechanism, or whether expansion should remain research.

**Inputs.** Phases A-E.

**Deliverables.**
- An assessment of the four eligibility framings against what earlier phases established.
- A recommendation on whether to proceed beyond research.
- If proceeding: the minimum specification a prototype would require.

**Blockers.** Phases A-E incomplete.

**Exit criteria.**
- A recommendation is reached, including - legitimately - that expansion should not
  proceed.
- If proceeding, every trust assumption is enumerated and none is implicit.
- No destination network is named at this or any prior phase.

## Status of every phase

| Phase | Status |
| :--- | :--- |
| A - Trust model comparison | Not started. Blocked on Issue #1. |
| B - Canonical representation specification | Not started. Blocked on Phase A. |
| C - Supply accounting model | Not started. Blocked on Phases A-B and Issue #2. |
| D - Messaging abstraction research | Not started. Blocked on Phases A-C. |
| E - Liquidity fragmentation simulation design | Not started. Blocked on Phases A-D. |
| F - Prototype eligibility review | Not started. Blocked on Phases A-E. |

No phase has begun. No deliverable in this document exists. A legitimate outcome of this
plan is the conclusion that expansion should not be built.

## Open design issues

- [#1 - Compare canonical representation trust models](https://github.com/Rearctor/expansion/issues/1)
- [#2 - Define supply and liquidity accounting across expansion venues](https://github.com/Rearctor/expansion/issues/2)
