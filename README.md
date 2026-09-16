# Rearctor Expansion

**Status: Concept / Research**

This repository documents open research. Nothing described here is implemented,
deployed, or scheduled. No specific networks are supported, and no bridging
infrastructure exists.

## Overview

Expansion explores how a reaction that originates on Arc could extend its
post-ignition presence into additional ecosystems.

<div align="center">

**Arc Reaction → Ignition → Uniswap V4 liquidity → Expansion → Additional ecosystems**

</div>

This is a research direction, not a feature. The questions below are open, and several
of them may not have satisfactory answers.

## Why expansion comes after ignition

A reaction before ignition is a bonding curve with a single USDC-denominated market
and a fixed threshold. Its price is a function of that curve, and its progress is
measured against a constant. Extending that state across ecosystems would mean
fragmenting the very market that determines when the reaction graduates.

After ignition, the position is different. The reaction has migrated to a full-range
Uniswap V4 position at the final bonding-curve price, at least 20% of initial supply
is in that pool, and the liquidity is permanent. There is an established market to
extend from, rather than a price-discovery process to interrupt.

For this research direction, expansion is considered strictly post-ignition so that
cross-ecosystem state is not introduced into the pre-ignition graduation path.

## Design considerations

- **Canonical representation.** Which deployment of a token is authoritative, and how
  is that established rather than asserted?
- **Supply integrity.** A reaction begins with a fixed initial supply. Any extension
  must not make that figure ambiguous.
- **Fee accounting.** Rearctor's 30/70 distribution applies to the migrated pool.
  How would it be accounted for across venues?
- **Lifecycle boundaries.** Which post-ignition properties - permanence, immutable fee
  configuration - must hold in an extended context, and which cannot?
- **Trust surface.** Any extension mechanism introduces assumptions that the native
  lifecycle does not have. Those assumptions need to be stated explicitly.

## Liquidity considerations

The migrated Uniswap V4 position has no principal-withdrawal function through
Rearctor. The underlying migrated liquidity cannot later be removed by the token
creator through Rearctor's contracts, so this research treats any additional venue as
distinct from the permanent migrated position.

That raises fragmentation directly. Multiple venues for the same asset divide
depth, and divided depth degrades execution on each. Whether expansion produces a
net improvement is an open question, not an assumption.

Note also that the hook preserving Rearctor's directional trading fees applies to the
Rearctor-managed migrated pool. Other pools for the same token are outside that hook's
scope.

## Cross-chain research

Areas under examination:

- cross-chain liquidity
- canonical asset representation
- messaging
- liquidity fragmentation
- fee accounting
- post-ignition expansion rules

No network has been selected. No bridging or messaging infrastructure is built,
integrated, or committed to. This section names research areas only.

## Open questions

- Does extending a reaction beyond its origin improve outcomes for holders, or does
  fragmentation outweigh the added reach?
- How can a canonical representation be established without introducing a privileged
  party?
- What messaging assumptions are acceptable for an asset whose native lifecycle makes
  no such assumptions?
- How should fee accounting work when revenue originates outside the Rearctor-managed
  pool?
- Which expansion rules, if any, could be fixed at launch rather than decided later?
- Is there a form of expansion that preserves the determinism of the native lifecycle,
  or is some determinism necessarily given up?

## Links

[Website](https://rearctor.io) · [Docs](https://rearctor.io/docs) · [GitHub](https://github.com/Rearctor) · [X](https://x.com/JoinRearctor) · [Telegram](https://t.me/rearctor)
