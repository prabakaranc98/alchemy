# Independent Causal Mechanisms / Modularity

**Layer:** Theory · **Role:** the backbone — *what is movable at all*

## What it is
The **Independent Causal Mechanisms (ICM)** principle says the data-generating process factorizes into **autonomous modules** that don't inform or influence each other. Change one mechanism (e.g. lighting in an image) and the others (object identity, pose) stay fixed. A representation that mirrors this factorization has parts you can swap, reuse, and recombine independently.

## Why it matters for Alchemy
This is the load-bearing assumption of the whole project. **If a capability corresponds to an independent mechanism, it's a module you can extract and graft. If it's entangled with everything else, it won't come loose** — no mapping trick will save you. ICM reframes "can we transfer capability X?" as "**is X modular in this model?**" That's why the central bet says the binding constraint is *modularity, not compute*.

ICM is deliberately chosen over the [Platonic Representation Hypothesis](platonic-representation.md) as the backbone: PRH's strong "everything converges" form is contested, while ICM makes a *weaker, testable* claim — *some* structure is modular, and modular structure transfers.

## The key mechanic
- Mechanisms are **independent**: knowing one tells you nothing about the others (no shared parameters).
- They're **reusable**: the same mechanism reappears across tasks/domains (this is what makes transfer possible).
- **Interventions are local**: editing one module leaves the rest invariant — exactly the property a clean "transplant" needs.

## The catch
- Real networks only *approximately* factorize; few capabilities are perfectly modular.
- ICM is a principle, not a detector — you still need a *measure* of modularity to predict transferability (Open Question 8). Building that measure is part of the contribution.

## References
- Parascandolo, Schölkopf et al., *Learning Independent Causal Mechanisms*, `1712.00961`
- Schölkopf et al., *Toward Causal Representation Learning*, `2102.11107`

## Related
[nonlinear-ica](nonlinear-ica.md) (when a module is *identifiable*) · [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md) (the practical extractor) · [platonic-representation](platonic-representation.md) (the rejected stronger claim)
