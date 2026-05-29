# Knowledge fusion

Knowledge fusion, in the FuseLLM lineage, combines the capabilities of several existing models into one at the level of capability and representation. It is architecture-agnostic and is framed explicitly as an alternative to training from scratch. Unlike [merging](model-merging.md) it does not require weights of a common lineage; it fuses what models do rather than their parameters.

This is the closest existing frame to the project, but with an addition that neither it nor merging names, which is downsizing. Knowledge fusion combines peers into a peer, whereas this project draws a capability from a flagship into a sub-100M specialist. That difference is the contribution space, and the reason the project adopts the term compact knowledge fusion: capability-level fusion that also reduces scale. FuseLLM and its successors supply the vocabulary and the thesis that fusion is an alternative to pretraining; the project adds the collapse in dimension, the [transport map](transport-ot-gw.md), and the modularity-based [theory](icm-modularity.md).

The mechanism aligns and combines the models' output distributions or representations over a corpus and then trains a target model to absorb the fused signal; being architecture-agnostic, it admits teachers that differ from the student, which weight merging does not. Two limitations follow. The original method fuses into a model of comparable size, so the flagship-to-small gap is not addressed there, and closing it is what the project's mapping and substrate work targets. And fusing distributions can still inherit the fidelity gap of [distillation](distillation.md).

**References.** Wan et al., *Knowledge Fusion of LLMs*, `2401.10491`.

**Related.** [model-merging](model-merging.md), [distillation](distillation.md), [icm-modularity](icm-modularity.md).
