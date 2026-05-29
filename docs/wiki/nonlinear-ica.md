# Nonlinear ICA / Identifiability

**Layer:** Theory · **Role:** *when* a feature is real and transportable

## What it is
**Independent Component Analysis (ICA)** recovers statistically independent source signals from mixed observations. Classic linear ICA is *identifiable* (you can recover the true sources up to permutation/scale). **Nonlinear ICA** is not — without extra structure, infinitely many "independent" decompositions explain the same data. Modern results (Hyvärinen et al.; **iVAE**) restore identifiability by conditioning on **auxiliary variables** (time, labels, environment), making the recovered factors provably the true ones.

## Why it matters for Alchemy
This is the **formal bridge between "modular mechanism" ([ICM](icm-modularity.md)) and "a feature I can actually move."** Identifiability is the difference between *a* direction that happens to correlate with a behavior and *the* direction that is the behavior's genuine source. You want to transplant the latter — the former won't transfer reliably.

**Sparse autoencoders are the practical, overcomplete descendant** of this theory: a sparsity prior is the structural assumption that (heuristically) recovers monosemantic, more nearly identifiable features ([dictionary-sae-crosscoder](dictionary-sae-crosscoder.md)).

## The key mechanic
- Independence + an auxiliary/structural assumption (sparsity, nonstationarity, conditioning) ⇒ the latent factors are recoverable up to trivial transforms.
- "Up to trivial transforms" is exactly what a [transport map](transport-ot-gw.md) is meant to undo across two models — identifiability tells you a *consistent* set of factors exists to map between.

## The catch
- Identifiability guarantees are strong on paper, fragile in practice; LLM features are only approximately sparse/independent.
- SAEs inherit this fragility: feature splitting, dead features, and "is this one real feature or two?" ambiguity are open problems.

## References
- Hyvärinen & Morioka, *Nonlinear ICA via auxiliary variables* (2016, 2019)
- Khemakhem et al., *Variational Autoencoders and Nonlinear ICA (iVAE)* (2020)

## Related
[icm-modularity](icm-modularity.md) (the principle this formalizes) · [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md) (the practical tool) · [pca-svd](pca-svd.md) (the linear, non-independent baseline)
