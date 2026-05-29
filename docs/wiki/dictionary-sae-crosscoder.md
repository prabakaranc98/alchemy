# Dictionary route — SAEs & Crosscoders

**Layer:** Extract (route) · **Role:** the interpretable, *verifiable* way to turn a capability into an object

## What it is
**Sparse Autoencoders (SAEs)** decompose a model's activations into a large, overcomplete dictionary of **sparse, roughly monosemantic features** ("this atom fires on legal language," "this one on Python"). A **crosscoder** is an SAE trained *jointly across two or more models or layers*, so its features live in a **shared space** — which makes them candidates for **transplant** from one model to another.

## Why it matters for Alchemy
This is the **extract** verb at its best, and the *only* route that lets you **verify what actually moved**. You can name the feature, watch it fire, ablate it, and check it survived the transfer. For narrow specialties, that interpretability is worth more than raw performance. Crosscoders are the natural tool for the headline recipe's first step: *crosscoder extracts → [OT/GW maps](transport-ot-gw.md) → [JEPA installs](predictive-jepa.md) → [OPD shapes](on-policy-distillation.md).*

For Exp 0, **public SAEs remove the training cost** — e.g. Gemma + **Gemma Scope** gives you a pre-trained dictionary on a same-family teacher, so you can go straight to choosing and transplanting a feature.

## The key mechanic
- Encoder maps activations → sparse codes; decoder columns are the **feature directions**. A capability ≈ one or a few decoder columns.
- Cross-architecture crosscoder = train one dictionary that reconstructs *both* models' activations ⇒ a feature index that means the same thing in both.
- Transplant = take the teacher's feature direction, [map](transport-ot-gw.md) it into student space, install it.

## The catch
- **Only some feature classes transfer** — not every "feature" is a real, transportable mechanism (see [nonlinear-ica](nonlinear-ica.md) on identifiability).
- A genuinely *cross-architecture* crosscoder is itself a research problem (different tokenizers, depths, widths). For Exp 0, prefer same-family + public SAE; defer the hard cross-arch crosscoder.
- SAE pathologies: feature splitting, dead features, reconstruction-vs-sparsity tradeoff.

## References
- Lindsey et al., *Crosscoders* (Anthropic, 2024); Gemma Scope SAEs (DeepMind, 2024)
- Cross-architecture diffing, `2602.11729`; feature transfer via stitching, `2506.06609`

## Related
[nonlinear-ica](nonlinear-ica.md) (when a feature is real) · [pca-svd](pca-svd.md) (the linear baseline extractor) · [control-steering](control-steering.md) (installing a single extracted direction) · [transport-ot-gw](transport-ot-gw.md) (mapping the feature across the gap)
