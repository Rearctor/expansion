# Trust Models

**Status: Concept / Research.** Comparison only. **No model is recommended, selected, or
preferred.** No destination network, bridge, or messaging provider is named.

## Framing

The native Arc lifecycle requires trusting no one. Parameters are fixed at deployment,
graduation is deterministic, migration is atomic, liquidity is permanent because no
withdrawal function exists.

**Every model below breaks that.** Each introduces at least one party or assumption the
native lifecycle does not have. The purpose of this document is to state precisely
*what* each introduces, so the cost is visible rather than absorbed.

The comparison uses four axes:

- **Added trusted party** - who must behave correctly, beyond the two chains.
- **Supply invariant** - whether it holds by construction or by mechanism.
- **Failure mode** - what breaks first, and what holders experience.
- **Recoverability** - whether the situation is repairable after failure.

---

## 1. Canonical bridge model

A designated bridge is the authoritative path. It holds origin assets and issues
representations.

| Axis | Assessment |
| :--- | :--- |
| Added trusted party | The bridge operator or its validator set |
| Supply invariant | **Mechanism.** Holds while the bridge is honest and solvent |
| Failure mode | Representations exceed backing; remote token depegs from claim |
| Recoverability | Poor. Backing is gone; remote holders hold an unbacked claim |

**Note.** Concentrating the path also concentrates the failure. A single canonical
bridge is the simplest to reason about and the largest single point of failure.

---

## 2. Lock and mint

Origin tokens are locked in a contract on Arc; equivalent tokens minted at the
destination. Reversing burns the representation and unlocks the origin.

| Axis | Assessment |
| :--- | :--- |
| Added trusted party | Whoever authorises mints - typically a validator set or light client |
| Supply invariant | **Mechanism.** Global total is fixed only if mints strictly correspond to locks |
| Failure mode | Unauthorised mint. Representations exceed locked backing |
| Recoverability | Poor for over-mint. Locked origin supply is insufficient for outstanding claims |

**Interaction with permanent liquidity.** Locked supply must come from circulating
supply, not the migrated position - that has no withdrawal function. So locking reduces
effective circulating supply on Arc while adding supply elsewhere.

> **Open question.** Whether reducing Arc circulating supply to fund a remote
> representation is acceptable, given that Arc liquidity is fixed and cannot be topped
> up on demand.

---

## 3. Burn and mint

Origin tokens are burned; equivalent tokens minted at the destination. The asset moves
rather than being mirrored.

| Axis | Assessment |
| :--- | :--- |
| Added trusted party | Whoever attests the burn |
| Supply invariant | **Mechanism.** Global total fixed only if every mint follows a real burn |
| Failure mode | Mint without burn inflates global supply irreversibly |
| Recoverability | Very poor. Burned origin supply cannot be restored |

**Direct conflict with the protocol's clearest property.** Rearctor states a fixed
initial supply of 1,000,000,000. A burn-and-mint scheme makes Arc supply *decrease* over
time while asserting the global total is unchanged - an assertion that depends on a
mechanism, not on arithmetic.

---

## 4. Liquidity-based representation

No canonical link. An independent token exists at the destination, with paired liquidity
against the origin asset on both sides. Price alignment comes from arbitrage.

| Axis | Assessment |
| :--- | :--- |
| Added trusted party | **None structurally** - but arbitrageurs must be present and capitalised |
| Supply invariant | **No invariant claimed.** Destination supply is independent |
| Failure mode | Price divergence when arbitrage is absent or a path closes |
| Recoverability | Good. Nothing is stranded; the two markets simply price separately |

**The only model with no added trusted party**, and the only one that makes no global
supply claim. Whether it counts as expansion at all is arguable: the destination asset
is a different asset that happens to track.

> **Under research.** Whether "tracks by arbitrage" is a meaningful form of expansion or
> a different thing wearing the name. It is the most honest model and possibly the least
> satisfying.

---

## 5. Message-verified representation

A general messaging layer carries verified state; the destination mints or unlocks based
on verified origin messages.

| Axis | Assessment |
| :--- | :--- |
| Added trusted party | The messaging layer's security model - validators, light client, or proof system |
| Supply invariant | **Mechanism.** Holds while messages cannot be forged |
| Failure mode | Forged or replayed message produces unbacked supply |
| Recoverability | Poor. Same as lock-mint, with a larger surface |
| Liveness | Additional failure mode: a halted layer freezes representation without corrupting it |

**Distinct property:** the halt case is a *safe* failure. State freezes rather than
diverging. That is materially better than a solvency failure, and worth weighing.

---

## Comparison

| Model | Added trusted party | Supply invariant | Worst failure | Recoverable |
| :--- | :--- | :--- | :--- | :--- |
| Canonical bridge | Bridge operator | Mechanism | Unbacked representations | No |
| Lock / mint | Mint authoriser | Mechanism | Over-mint beyond backing | No |
| Burn / mint | Burn attester | Mechanism | Irreversible global inflation | No |
| Liquidity-based | None structural | None claimed | Price divergence | Yes |
| Message-verified | Messaging security | Mechanism | Forged message | No; halt case is safe |

**One pattern is visible:** every model that preserves a global supply invariant does so
by **mechanism**, and every mechanism failure is unrecoverable. The only model with a
recoverable failure mode is the one that claims no invariant at all.

That may be the most important observation in this document. It suggests the choice is
not "which mechanism is safest" but "is a mechanism-guaranteed supply invariant worth
having at all, given that its failure is terminal".

> **Open question.** Whether a mechanism-guaranteed invariant should ever be presented
> alongside Arc's construction-guaranteed one. The working view is no - they are
> different kinds of claim and conflating them misleads.

## No recommendation

**No model is selected.** Each is recorded with its costs. Selection would require
deciding what failure mode is acceptable, which is not a research question.

## Related documents

- [Architecture](../docs/architecture.md)
- [RFC 0001 - Canonical representation](../rfcs/0001-canonical-representation.md)
- [Liquidity fragmentation](liquidity-fragmentation.md)
