# Model Merging

**Layer:** Framing / baseline · **Role:** the adjacent field — and its hard ceiling

## What it is
Combining models **in parameter space**, usually *same-lineage* (same base, different finetunes): **weight averaging**, **task arithmetic** (add/subtract task vectors), **TIES** (trim + elect signs to cut interference), **DARE** (drop-and-rescale), **evolutionary merge** (search merge coefficients). No training data needed — just arithmetic on weights.

## Why it matters for Alchemy
Merging is the *recognizable* umbrella next door, but it's **weight-space and same-family** — it doesn't *downsize* and doesn't cross architectures, so it's not the project's frame ([knowledge-fusion](knowledge-fusion.md) is). It matters as a **source of techniques and a hard constraint**:

> **Merging saturates and can't exceed the union of its parents** — interference grows with the number of models, with a documented ceiling around **4–6 models** ("Why Do More Experts Fail?", `2505.21226`).

This ceiling is a central worry at the *capability* level (Open Question 5): how many transplanted capabilities co-install before interference dominates? And it's why exceeding the source needs [RL](rl-grpo.md), not more merging.

## The key mechanic
- Task vector = `θ_finetuned − θ_base`; merge = base + weighted sum of task vectors.
- Interference reduction: sign election (TIES), sparsification (DARE), or [low-rank SVD subspaces](pca-svd.md).
- Only wins for **combinations no single parent has** — never beyond their union.

## The catch
- Same-architecture / same-init assumption breaks for flagship→tiny (different dims) — that's where [transport/GW](transport-ot-gw.md) is needed instead.
- The 4–6 ceiling caps naive composition.

## References
- Model-merging survey (ACM Computing Surveys 2026), `2408.07666`
- *Why Do More Experts Fail?*, `2505.21226`; task arithmetic (Ilharco et al., 2023)

## Related
[knowledge-fusion](knowledge-fusion.md) (the broader, cross-arch frame) · [pca-svd](pca-svd.md) (SVD subspaces reduce merge interference) · [control-steering](control-steering.md) (task vectors as install) · [quality-diversity](quality-diversity.md) (evolving merge recipes)
