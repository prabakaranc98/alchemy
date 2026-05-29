# Platonic Representation Hypothesis (PRH)

**Layer:** Theory · **Role:** the *tempting but contested* claim — know why we don't lean on it

## What it is
The **Platonic Representation Hypothesis** claims that as models scale and improve, their internal representations **converge toward a shared, modality-agnostic statistical model of reality** — a "platonic" representation different architectures and even different modalities all approximate.

## Why it matters for Alchemy
If PRH's strong form were true, transfer would be *easy*: any two good models already speak nearly the same language, so a thin map suffices. **Alchemy deliberately does *not* bet on this.** The strong form is empirically contested:

- Measured "convergence" is **confounded by network scale**, not a universal law.
- Convergence is **strongest for generic features and weakest exactly for the narrow, specialized representations** Alchemy wants to move (the "Aristotelian" rebuttal, `2602.14486`).

So the honest stance: **the bridge between two models carries *some* shared structure, but assume it's partial — especially for specialties.** Whether a given capability crosses cleanly is the experiment, not an assumption. This is why [ICM/modularity](icm-modularity.md) is the chosen backbone instead.

## The key mechanic
- Evidence *for*: representational-similarity metrics (CKA, mutual-NN) rise with capability across vision/language models.
- Evidence *against*: those metrics track scale; specialized/narrow features show the *least* alignment — the regime Alchemy operates in.

## The catch
- Treat any "models are aligned" result as a *measurement at a scale*, not a guarantee. Build the [transport map](transport-ot-gw.md) to *measure* alignment per capability rather than assuming it.

## References
- Huh et al., *The Platonic Representation Hypothesis*, `2405.07987`
- Aristotelian rebuttal / scale-confound critique, `2602.14486`

## Related
[icm-modularity](icm-modularity.md) (the sturdier backbone) · [transport-ot-gw](transport-ot-gw.md) (how we *measure* alignment instead of assuming it)
