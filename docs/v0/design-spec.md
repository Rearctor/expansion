# Rearctor Expansion - v0 Design Specification

## Status

**Concept / Research.** This is a draft design target for open research, not a
production specification.

Nothing described here is implemented, deployed, scheduled, or audited. No bridging
exists. No messaging integration exists. **No destination network is named, selected, or
implied anywhere in this repository. No cross-chain model is selected.** There is no
commitment that expansion will be built.

Builds on, and does not replace:

- [docs/architecture.md](../architecture.md)
- [docs/expansion-lifecycle.md](../expansion-lifecycle.md)
- [rfcs/0001-canonical-representation.md](../../rfcs/0001-canonical-representation.md)
- [research/trust-models.md](../../research/trust-models.md)
- [research/liquidity-fragmentation.md](../../research/liquidity-fragmentation.md)
- [specs/expansion-manifest.schema.json](../../specs/expansion-manifest.schema.json)

Every numeric figure here that is not a Rearctor protocol constant is illustrative only.
Not protocol defaults or committed parameters.

## Scope

### Proposed for v0

- A manifest format that forces every trust assumption to be recorded explicitly.
- Origin-canonical framing: the Arc reaction is authoritative by definition.
- A required distinction between invariants guaranteed by **construction** and by
  **mechanism**.
- Opaque destination references, so no network is named.
- Expansion scoped strictly post-ignition.

### Under research

- Which representation model, if any. See Issue #1. **No winner is chosen.**
- Supply and liquidity accounting across venues. See Issue #2.
- Expansion eligibility framing.
- Messaging assumptions and their failure modes.
- Whether reversibility is supported at all.

### Non-goals

- Naming, selecting, evaluating or implying any destination network.
- Selecting a bridging or messaging provider.
- Any pre-ignition expansion.
- Relocating the permanent Arc migrated position.
- Any change to Arc initial supply, fee configuration, ignition threshold, migration, or
  migrated principal.
- Governance mechanisms.

## Native lifecycle boundary

These hold on Arc by construction and are the benchmark for any expansion design:

| Property | Mechanism |
| :--- | :--- |
| Fixed initial supply | 1,000,000,000, set at Spark |
| Immutable trading fees | 1%-10%, fixed at deployment |
| Deterministic graduation | 5,042 USDC, no early graduation |
| Atomic migration | Graduation and migration in one transaction |
| Permanent liquidity | No principal-withdrawal function in Rearctor's contracts |
| Untaxed transfers | Fees apply to trading, not to moving tokens |

**Every one is a statement about a single chain.** None survives transplantation
automatically. That gap - between what is true on Arc and what is true of the asset - is
the subject of this specification.

## Expansion eligibility

A gate, not an event. Four framings, none selected:

| Framing | Problem |
| :--- | :--- |
| Automatic | Expansion becomes a protocol property with no opt-out |
| Creator-elected | A post-launch decision buyers could not inspect at purchase |
| Declared at Spark | Requires committing before the mechanics are knowable |
| Threshold-gated | Another threshold decision, with the same objections as elsewhere |

Declared-at-Spark fits the protocol's pre-declaration philosophy best and requires a
creator to commit to something that does not exist. **No option is selected.**

## Canonical representation

**Working position:** the Arc reaction is canonical by definition; anything elsewhere is
explicitly a representation.

Rationale: the origin is where the guarantees are true by construction, a representation
claiming equivalence obscures its own trust assumptions, and a single origin keeps supply
arithmetic checkable.

**Cost:** representations are permanently second-class, which may make expansion less
attractive than it first appears. See Issue #1.

Identity resolution across ecosystems is unresolved. Metadata convention alone is
insufficient - it is the impersonation vector.

## Supply invariant

The manifest requires two fields together, and the pairing is the point:

| Field | Values |
| :--- | :--- |
| `claim` | `global-total-fixed`, `per-chain-total-fixed`, `no-invariant-claimed` |
| `guaranteed_by` | `construction`, `mechanism`, `unspecified` |

**An invariant guaranteed by construction and one guaranteed by mechanism are different
kinds of claim.** Arc's fixed supply is arithmetic. A cross-venue supply invariant holds
only while a mechanism behaves correctly. Presenting them identically misleads, and v0
treats conflating them as a specification defect.

Three positions on accounting, none selected:

- **A.** Arc supply is the supply; representations are claims against locked origin
  supply. Checkable on Arc alone; requires the locking mechanism to be honest.
- **B.** Global total fixed across all representations. Checkable only by observing every
  ecosystem; truth depends on the weakest component.
- **C.** No global invariant claimed. Honest and trivially checkable; "fixed supply"
  stops being sayable about the asset.

See Issue #2.

## Messaging

Any representation that is not purely liquidity-based depends on messages crossing
between ecosystems. Messaging introduces assumptions the native lifecycle does not have:

- messages cannot be forged;
- messages are delivered, or their absence is detectable;
- replay is prevented;
- the layer's security model is understood by whoever relies on it.

**One distinct property worth weighing:** a halted messaging layer freezes the
representation rather than corrupting it. A safe failure is materially better than a
solvency failure.

**Unresolved:** whether identity resolution must be verifiable on the destination side
without reading Arc. If it must, attestation is unavoidable and the messaging trust
assumption follows.

## Trust model

**No model is selected.** Five are compared in
[research/trust-models.md](../../research/trust-models.md):

| Model | Added trusted party | Supply invariant | Worst failure | Recoverable |
| :--- | :--- | :--- | :--- | :--- |
| Canonical bridge | Bridge operator | Mechanism | Unbacked representations | No |
| Lock / mint | Mint authoriser | Mechanism | Over-mint beyond backing | No |
| Burn / mint | Burn attester | Mechanism | Irreversible global inflation | No |
| Liquidity-based | None structural | None claimed | Price divergence | Yes |
| Message-verified | Messaging security | Mechanism | Forged message | No; halt case is safe |

**The pattern that matters:** every model preserving a global supply invariant does so by
mechanism, and every mechanism failure is unrecoverable. The only model with a
recoverable failure mode claims no invariant at all.

This suggests the question is not "which mechanism is safest" but "is a
mechanism-guaranteed supply invariant worth having, given that its failure is terminal".
See Issue #1.

The manifest requires `trust_assumptions` with at least one entry. An empty array would
claim expansion is trustless, which no model supports.

## Additional liquidity venues

The Arc migrated position has no principal-withdrawal function through Rearctor.
Therefore **expansion cannot move liquidity; it can only add it.** Any presence elsewhere
is separately funded and separately deep.

Consequences:

- Arc depth is a floor that expansion cannot reduce. Protective.
- Arc depth is also a ceiling that cannot be raised to meet remote demand.
- A successful expansion could leave the canonical venue holding immobile depth in a
  market that has moved.

**The question that decides it:** is remote liquidity *additional* capital that would
never have come to Arc, or *diverted* capital that would otherwise have traded there?
Additional makes the trade-off arguable; diverted makes it strictly worse. This is an
empirical question about markets that do not exist. See Issue #2.

## Fee accounting

Rearctor's 30/70 split applies to the Rearctor-managed migrated pool, preserved by a
hook. Other pools for the same token are outside that hook's scope - true already on Arc,
and far more consequential across venues.

Unresolved: whether trading elsewhere produces protocol revenue at all; if not, whether
the 30/70 split remains a property of the asset or only of one venue; if so, what the
return path to Arc accounting would be.

The manifest records this as `undetermined` rather than assuming an answer.

## Failure modes

| Failure | Consequence |
| :--- | :--- |
| Representation mechanism compromised | Supply integrity breaks; the global fixed-supply claim becomes false |
| Remote venue liquidity collapses | Remote holders stranded; divergence with no arbitrage path |
| Messaging layer halts | Representation frozen; state diverges silently |
| Price divergence without arbitrage | Two prices for one asset; "the price" becomes ambiguous |
| Fee accounting diverges | The 30/70 split holds on Arc and not elsewhere |

Each is a way the native guarantees stop being true of the asset while remaining true on
Arc.

## Recovery assumptions

v0 makes **no recovery guarantee**. Recorded honestly:

- The Arc side is unaffected by any remote failure, by construction.
- Holders of a remote representation may not be.
- Returning supply would have to be absorbed by Arc liquidity that is fixed and cannot be
  topped up on demand.
- Four of the five trust models have unrecoverable worst-case failures.

**Unresolved:** whether expansion should be reversible at all, and what reversibility
would mean given the constraint above. A design where remote holders can be stranded with
no path back must say so plainly rather than assume the case away.

## Open design issues

- [#1 - Compare canonical representation trust models](https://github.com/Rearctor/expansion/issues/1)
- [#2 - Define supply and liquidity accounting across expansion venues](https://github.com/Rearctor/expansion/issues/2)

Both block v0. No representation model can be specified until Issue #1 resolves, and no
supply invariant can be claimed until Issue #2 does.

## Related documents

- [Architecture](../architecture.md)
- [Expansion lifecycle](../expansion-lifecycle.md)
- [RFC 0001 - Canonical representation](../../rfcs/0001-canonical-representation.md)
- [Trust models](../../research/trust-models.md)
- [Liquidity fragmentation](../../research/liquidity-fragmentation.md)
- [Implementation plan](implementation-plan.md)
