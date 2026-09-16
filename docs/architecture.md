# Expansion - Architecture

**Status: Concept / Research.** Open research only. Nothing here is implemented,
deployed, scheduled, or committed to. **No destination network is named, selected, or
implied anywhere in this repository**, and no bridging or messaging infrastructure
exists.

## What is being asked

A reaction originates on Arc. After ignition it holds a permanent full-range Uniswap V4
position at the final bonding-curve price, with at least 20% of initial supply in it.

Expansion asks: **could that asset acquire presence in an additional ecosystem without
giving up the properties that made the original lifecycle trustworthy?**

The honest answer at this stage is that it is not known, and several of the sub-problems
below may not have satisfactory answers.

## What the native lifecycle guarantees

These are the properties any expansion design is measured against. They hold on Arc by
construction:

| Property | Mechanism |
| :--- | :--- |
| Fixed initial supply | 1,000,000,000, set at Spark |
| Immutable trading fees | 1%-10%, fixed at deployment |
| Deterministic graduation | 5,042 USDC, no early graduation |
| Atomic migration | Graduation and migration in one transaction |
| Permanent liquidity | No principal-withdrawal function in Rearctor's contracts |
| Untaxed transfers | Fees apply to trading, not to moving tokens |

**Every one of these is a statement about a single chain.** None of them survives
transplantation automatically. That is the central difficulty.

## Where the difficulty concentrates

### Supply integrity

"Fixed initial supply of 1,000,000,000" is checkable on Arc. Once a representation
exists elsewhere, the question becomes: what is the supply *of the asset*, as opposed
to the supply on any one chain?

A lock-and-mint scheme keeps a global total constant only if the lock is honoured. A
burn-and-mint scheme keeps it constant only if burns are real and mints are
correspondingly authorised. In both cases the invariant moves from *arithmetic* to
*trusted*, and that is a category change, not a parameter change.

> **Open question.** Whether a supply invariant that depends on a mechanism behaving
> correctly can be described in the same terms as one that is true by construction. The
> working view is that it cannot, and that expansion documentation must not conflate
> them.

### Canonical representation

If the same asset exists in two places, which is authoritative? See
[RFC 0001](../rfcs/0001-canonical-representation.md).

### Fee accounting

Rearctor's 30/70 split applies to the Rearctor-managed migrated pool, preserved there by
a hook. Other pools for the same token are outside that hook's scope - which is true
today on Arc, and becomes far more consequential if additional venues exist elsewhere.

Questions with no current answer: does trading in another ecosystem produce protocol
revenue at all? If not, is the 30/70 split still a property of the asset or only of one
venue? If so, how does revenue return to an accounting system on Arc?

### Trust surface

The native lifecycle requires trusting no one: parameters are fixed, migration is
unconditional, liquidity is permanent. **Every expansion mechanism introduces at least
one party or assumption that the native lifecycle does not have.**

This is not an argument against expansion. It is a requirement that the added
assumption be stated explicitly rather than absorbed silently. See
[trust-models.md](../research/trust-models.md).

## Structural constraint: liquidity cannot move

The migrated Uniswap V4 position has no principal-withdrawal function through Rearctor.
The creator cannot later remove the underlying migrated liquidity through Rearctor's
contracts.

Consequence for this research: **any additional venue implies additional liquidity, not
relocated liquidity.** Expansion cannot be a migration. It can only be an addition, and
an addition has to be funded from somewhere.

This makes fragmentation a first-order concern rather than a side effect. See
[liquidity-fragmentation.md](../research/liquidity-fragmentation.md).

## Scope boundaries

**In scope:** canonical representation, supply integrity, messaging assumptions,
additional liquidity venues, fragmentation, fee accounting, trust surfaces,
post-ignition expansion rules.

**Out of scope:** naming or evaluating specific networks; selecting a bridging or
messaging provider; any pre-ignition expansion; governance mechanisms; any claim about
whether expansion will be built.

## Why post-ignition only

Pre-ignition, a reaction is a single USDC-denominated market whose reserve level
determines graduation. Introducing a second venue during that phase would fragment the
exact market that determines when the threshold is reached, and make progress toward
5,042 USDC dependent on cross-ecosystem state.

For this research direction, expansion is therefore considered strictly post-ignition so
that cross-ecosystem state is not introduced into the pre-ignition graduation path.
This is a scoping decision for the research, not a claimed protocol theorem.

## Related documents

- [Expansion lifecycle](expansion-lifecycle.md)
- [RFC 0001 - Canonical representation](../rfcs/0001-canonical-representation.md)
- [Trust models](../research/trust-models.md)
- [Liquidity fragmentation](../research/liquidity-fragmentation.md)
