# Expansion Lifecycle

**Status: Concept / Research.** No destination network is named or implied. Nothing
here is implemented, deployed, or scheduled.

## Conceptual sequence

```
  Arc Reaction
       |
       v
   Ignition                 atomic graduation + migration at 5,042 USDC
       |
       v
  Permanent V4 position     >= 20% of initial supply, no principal withdrawal
       |
       v
  Expansion eligibility     a gate, not an event  <-- entirely unspecified
       |
       v
  Additional ecosystem      an additional venue, never a relocation
       presence
```

The first three stages exist today. The last two are the research.

## Stage by stage

### Arc Reaction

Fixed supply, immutable fees, USDC-denominated curve. Single chain, single market. No
expansion concept applies.

### Ignition

Graduation and migration execute in one transaction at the final bonding-curve price.

**Why this is the earliest possible boundary.** Before ignition, reserve level
determines graduation. A second venue would mean progress toward 5,042 USDC depends on
state that is not on Arc. That is a materially different protocol.

### Permanent V4 position

The reaction now has an established market at a discovered price, with at least 20% of
supply in a full-range position that cannot be withdrawn through Rearctor.

Two consequences for expansion:

1. **There is a reference price.** Pre-ignition there is only a curve; post-ignition
   there is a market with outside participants. Any cross-ecosystem representation needs
   a price reference, and this is the first point one exists.
2. **The liquidity is immobile.** Expansion cannot draw on it. Additional presence must
   be funded separately.

### Expansion eligibility

A gate: which reactions, if any, may expand, and who decides.

This is the least-developed part of the research. Candidate framings:

| Framing | Description | Problem |
| :--- | :--- | :--- |
| Automatic | Every ignited reaction is eligible | Expansion becomes a protocol property with no opt-out |
| Creator-elected | Creator chooses post-ignition | A post-launch decision that buyers could not inspect at purchase |
| Declared at Spark | Eligibility fixed before first trade | Consistent with pre-declaration; requires deciding before it is knowable |
| Threshold-gated | Eligibility on observable criteria | Another threshold decision, with the same objections as elsewhere |

> **Open question.** All four are unsatisfying. Declared-at-Spark fits the protocol's
> existing philosophy best, and requires a creator to commit to something whose
> mechanics do not exist. No option is selected.

### Additional ecosystem presence

A representation of the asset exists in an additional ecosystem, with its own liquidity.

**Never a relocation.** The Arc position stays. Whatever exists elsewhere is additional,
separately funded, and separately deep.

Open in every respect: how the representation is created, what makes it canonical,
whether it generates protocol revenue, and what happens if the two venues' prices
diverge.

## What does not change

Regardless of any expansion design:

- initial supply on Arc remains fixed;
- the Arc trading-fee configuration remains immutable;
- the ignition threshold remains 5,042 USDC;
- the migrated Arc position remains permanent and non-withdrawable through Rearctor;
- wallet-to-wallet transfers on Arc remain untaxed.

Expansion must not require altering any of these. A design that does is out of scope by
construction.

## Reversibility

If a representation exists elsewhere and that ecosystem becomes unusable - the venue
dies, the mechanism fails, liquidity leaves - what happens to holders there?

The Arc side is unaffected by construction. Holders of the remote representation may
not be. A design where remote holders can be stranded with no path back needs to say so
plainly rather than assume the case away.

> **Under research.** Whether expansion should be reversible at all, and what
> reversibility would mean when the Arc liquidity that would have to absorb returning
> supply is fixed and cannot be added to on demand.

## Failure modes worth naming early

| Failure | Consequence |
| :--- | :--- |
| Representation mechanism compromised | Supply integrity breaks; the global fixed-supply claim becomes false |
| Remote venue liquidity collapses | Remote holders stranded; price divergence with no arbitrage path |
| Messaging layer halts | Representation frozen; state diverges silently |
| Price divergence without arbitrage | Two prices for one asset; "the price" becomes ambiguous |
| Fee accounting diverges | 30/70 holds on Arc and not elsewhere; the split stops being a property of the asset |

Each of these is a way the native lifecycle's guarantees stop being true of the asset as
a whole while remaining true on Arc. That gap - between what is true on Arc and what is
true of the asset - is the recurring theme of this research.

## Related documents

- [Architecture](architecture.md)
- [RFC 0001 - Canonical representation](../rfcs/0001-canonical-representation.md)
- [Trust models](../research/trust-models.md)
- [Liquidity fragmentation](../research/liquidity-fragmentation.md)
