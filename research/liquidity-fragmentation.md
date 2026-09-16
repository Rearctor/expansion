# Liquidity Fragmentation

**Status: Concept / Research.** No destination network is named. No conclusion is
reached.

## The structural fact

The migrated Uniswap V4 position on Arc has no principal-withdrawal function through
Rearctor. The creator cannot later remove the underlying migrated liquidity through
Rearctor's contracts.

Therefore **expansion cannot move liquidity. It can only add it.** Any presence
elsewhere is separately funded and separately deep.

This single constraint shapes everything below. In a protocol where liquidity could be
relocated, expansion would be a routing decision. Here it is an additional-capital
decision.

## Why fragmentation follows

Two venues for one asset divide trading across them. Each is shallower than a single
combined venue would be.

Depth is what determines execution quality. For a constant-product-style pool, price
impact rises roughly with trade size relative to depth - so halving depth more than
halves the size a trader can execute at a given tolerance.

Concretely: a trader who can move 1% of the asset's price with a given trade size in a
combined venue moves it more with the same trade in each of two split venues. The
aggregate depth figure looks unchanged; the *usable* depth is not.

## The cost side

**Worse execution at every venue.** Splitting depth degrades execution on each, for
identical total capital.

**Price divergence.** Two venues with independent order flow price independently. They
converge only when someone arbitrages, which requires a path, capital, and a spread
wide enough to pay for both.

**Ambiguous price.** "The price" needs qualification. Every consumer - a UI, a signal, a
strategy trigger - must decide which venue it means, and different choices produce
different answers.

**Arbitrage dependency.** Convergence is not automatic. It is a service performed by
parties who require compensation. If the path closes or the spread is too thin, the
venues drift.

**Fee accounting divergence.** Rearctor's 30/70 split is preserved on the migrated Arc
pool by a hook. Other pools are outside that hook's scope. Trading at another venue may
generate no protocol revenue at all - so the split stops being a property of the asset
and becomes a property of one venue.

## The benefit side

Fragmentation is a real cost. It is not automatically decisive.

**Access.** Participants who will not or cannot use Arc cannot trade the asset at all
today. Shallow access is not obviously worse than no access.

**Additional capital.** Expansion adds liquidity that would otherwise not exist. If
remote liquidity is genuinely new rather than diverted, aggregate depth rises even as
usable depth per venue falls.

**Local composability.** An asset present in another ecosystem can be used by that
ecosystem's protocols. That utility does not exist at any depth on Arc.

**Resilience.** Two venues do not fail together. Weak, given that one is canonical, but
not nothing.

## The question that decides it

**Is remote liquidity additional, or diverted?**

- **Additional** - new capital that would never have come to Arc. Aggregate depth rises;
  per-venue depth falls; the trade-off is real but arguable.
- **Diverted** - capital that would otherwise have traded on Arc. Aggregate depth
  unchanged; per-venue depth falls; strictly worse for everyone.

This is an empirical question about markets that do not exist, and nothing in this
research answers it.

> **Open question.** Whether the additional/diverted split can even be estimated before
> building. If it cannot, expansion is a bet on an unmeasured quantity.

## Asymmetry from immobile Arc liquidity

Ordinary fragmentation debates assume liquidity can rebalance. Here it cannot - at
least on the Arc side.

Consequences:

- Arc depth is a floor that cannot be reduced by expansion. Genuinely protective.
- Arc depth is also a ceiling that cannot be raised to meet remote demand.
- If the remote venue becomes dominant, Arc holds permanent liquidity for a market that
  has moved, and that liquidity cannot follow.

The last case deserves attention. A successful expansion could leave the canonical venue
holding immobile depth in a market that has migrated elsewhere - while remaining, by
definition, canonical.

## What would need measuring

For an informed decision rather than an intuition:

| Quantity | Why it matters |
| :--- | :--- |
| Depth-weighted price impact per venue | Direct measure of execution degradation |
| Cross-venue spread over time | Whether arbitrage is functioning |
| Share of remote volume that is arbitrage | Distinguishes genuine demand from convergence |
| Origin volume before and after | Direct test of additional vs diverted |
| Protocol revenue per venue | Whether the 30/70 split survives expansion |

None of these are observable before expansion exists, which is the methodological
problem: the decision must be made before the data exists.

## Current position

Fragmentation is a certain cost. The benefits are plausible but unquantified. Whether
expansion produces a net improvement is **an open question, not an assumption** - and
the framing "expansion is growth" is precisely the assumption this document declines to
make.

## Related documents

- [Architecture](../docs/architecture.md)
- [Expansion lifecycle](../docs/expansion-lifecycle.md)
- [Trust models](trust-models.md)
