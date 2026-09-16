# RFC 0001 - Canonical Representation

- **Status:** Draft / Research. Not accepted, not scheduled, not implemented.
- **Scope:** If the same asset exists in more than one ecosystem, what makes one
  instance authoritative.
- **Note:** No destination network is named, selected, or implied.

## Problem

On Arc, a reaction's token is unambiguous: one contract, one supply, one fee
configuration. "The token" is a specific object.

Once a representation exists elsewhere, "the token" needs a definition. Without one:

- price quotes are ambiguous;
- supply figures are ambiguous;
- integrations disagree about what they are integrating;
- a holder cannot tell whether what they hold is the asset or a claim on it.

Ambiguity here is not cosmetic. It is the difference between a fixed supply and an
unbounded one.

## Proposal: origin is canonical, and says so

The working position is that **the Arc reaction is canonical by definition**, and every
representation elsewhere is explicitly a representation.

Rationale:

- **The origin is where the guarantees are true.** Fixed supply, immutable fees,
  deterministic graduation, permanent liquidity - all Arc properties by construction.
  Whatever is elsewhere holds them only derivatively.
- **A representation that claims equivalence obscures its own trust assumptions.**
  Calling something "the token on chain X" hides the mechanism between it and the
  origin. Calling it "a representation of the Arc token" makes the mechanism visible.
- **Single origin keeps supply arithmetic checkable.** Arc supply is fixed at
  1,000,000,000 and remains so regardless of what exists elsewhere.

**Cost:** representations are permanently second-class, which may be unattractive to
the destination ecosystem and to holders there.

> **Open question.** Whether second-class status is acceptable, or whether it makes
> expansion pointless. If the goal is presence in another ecosystem, and presence there
> is explicitly a derivative claim, the value of expanding is less obvious than it first
> appears.

## Supply accounting under a single origin

Three positions, in decreasing strength:

### A. Arc supply is the supply; representations are claims

Global supply is 1,000,000,000 on Arc. A representation elsewhere is a claim against
locked origin supply, not new supply.

- **Checkable:** yes, on Arc alone.
- **Requires:** that the locking mechanism is honest. If it is not, representations
  exceed claims and the invariant is false in practice while true on Arc.

### B. Global total is fixed across all representations

Supply may exist in several places; the sum is constant.

- **Checkable:** only by observing every ecosystem simultaneously.
- **Requires:** correct accounting across all of them, permanently.
- **Weakness:** the invariant becomes a property of a *system*, not of a contract. Its
  truth depends on the weakest component.

### C. No global invariant is claimed

Each ecosystem's supply is stated separately; no aggregate is asserted.

- **Checkable:** trivially, per chain.
- **Honest**, and arguably the only position that does not overclaim.
- **Weakness:** "fixed supply" stops being sayable about the asset, which is one of the
  protocol's clearest properties.

The draft schema forces this choice to be recorded, alongside whether the invariant is
guaranteed by **construction** or by **mechanism**. That distinction is the load-bearing
part: an invariant that holds arithmetically and one that holds while a bridge behaves
are not the same claim and must never be presented identically.

> **Under research.** A, B, or C. A is the working assumption. C may be the most
> defensible.

## Identity across ecosystems

Whatever is canonical, an integrator needs to resolve "is this the same asset?".
Candidate mechanisms:

| Mechanism | Strength | Weakness |
| :--- | :--- | :--- |
| Origin-signed registry on Arc | Authoritative at origin | Remote consumers must read Arc |
| Deterministic address derivation | No lookup needed | Constrains destination deployment; collisions |
| Message-attested linkage | Works without reading origin | Inherits messaging trust assumptions |
| Metadata convention only | Trivial | Unauthenticated - anyone can claim linkage |

Metadata convention is clearly insufficient - it is the impersonation vector. The other
three trade off where the trust sits.

> **Open question.** Whether identity resolution must be verifiable on the destination
> side without reading Arc. If it must, some form of attestation is unavoidable, and the
> messaging trust assumption follows.

## What must remain true regardless

1. Arc initial supply remains 1,000,000,000 and is not altered by any representation.
2. Arc fee configuration remains immutable.
3. The Arc migrated position remains permanent and non-withdrawable through Rearctor.
4. No representation may mint origin supply.
5. Every representation's trust assumptions are stated, not implied.

## Alternatives considered

**Symmetric multi-home.** No canonical origin; all instances peer. Rejected in this
draft: without an origin there is no place where supply is checkable by construction,
and every guarantee becomes a property of a distributed system.

**Destination-canonical.** The representation becomes authoritative and Arc becomes
secondary. Rejected: it discards the guarantees that make a reaction inspectable in the
first place.

## Related documents

- [Architecture](../docs/architecture.md)
- [Expansion lifecycle](../docs/expansion-lifecycle.md)
- [Trust models](../research/trust-models.md)
