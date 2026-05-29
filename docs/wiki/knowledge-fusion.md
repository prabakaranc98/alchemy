# Knowledge Fusion

**Layer:** Framing · **Role:** the umbrella Alchemy files under

## What it is
**Knowledge fusion** (the **FuseLLM** lineage) combines the **capabilities** of multiple existing models into one — at the *capability/representation level*, **architecture-agnostic**, and explicitly framed as **the alternative to training from scratch**. Unlike [merging](model-merging.md), it doesn't require same-lineage weights: it fuses what models *do*, not their parameters directly.

## Why it matters for Alchemy
This is the **closest existing frame** to the project — but with one twist neither it nor merging names: **downsizing.** Knowledge fusion fuses peers into a peer; Alchemy pulls capability from a **flagship into a sub-100M specialist.** That gap is the contribution space, which is why the project coins:

> **Compact knowledge fusion** — capability-level fusion *that shrinks*.

FuseLLM and its successors give the vocabulary and the "alternative to pretraining" thesis; Alchemy adds the dimensional collapse, the [transport map](transport-ot-gw.md), and the modularity-based [theory](icm-modularity.md).

## The key mechanic
- Fuse by aligning and combining models' **output distributions / representations** over a corpus, then training a target model to absorb the fused signal.
- Architecture-agnostic ⇒ teachers can differ from the student (unlike weight merging).

## The catch
- Original FuseLLM fuses *into a same-size-ish* model; the **flagship→tiny dimensional gap** is unsolved there — exactly what Alchemy's [map](transport-ot-gw.md) and [substrate](../PRIMER.md) work targets.
- "Fuse the distributions" can still inherit [distillation's](distillation.md) fidelity gap.

## References
- Wan et al., *Knowledge Fusion of LLMs (FuseLLM)*, `2401.10491`

## Related
[model-merging](model-merging.md) (the weight-space cousin) · [distillation](distillation.md) (a fusion mechanism with limits) · [icm-modularity](icm-modularity.md) (what makes fusion possible)
