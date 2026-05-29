# Evaluation and contamination

Benchmark contamination refers to the leakage of public test sets into pretraining corpora, with the consequence that a high benchmark score may reflect memorisation rather than capability. Contamination-resistant evaluation measures capability with held-out, freshly constructed, or behaviourally probed instruments that the models cannot have seen.

The project's success criterion depends on trustworthy measurement, and is reported as a difference rather than an absolute: the score of the transferred student minus that of the distillation baseline, on a held-out or purpose-built evaluation rather than a published benchmark. Because both arms are run on the same evaluation, even an imperfect instrument gives a fair relative signal, provided it is not contaminated in a way that ceilings both arms; and because the student carries no teacher at inference, the evaluation must score the standalone model.

In practice one prefers purpose-built contrastive evaluations for the target capability, with held-out splits and fresh prompts, scored behaviourally — whether the generation exhibits the capability — rather than through a leaked multiple-choice set, and one reports the difference from the distillation baseline together with the difference from the untouched student, to establish whether anything moved at all. Two cautions apply. A purpose-built evaluation can be biased toward the mechanism that was installed, so it should be designed before the model is touched and a held-out split retained. And a small or negative difference is a valid result, indicating that the capability is not modular enough to move by the route attempted; the evaluation should not be adjusted until the difference becomes favourable.

**References.** Contamination findings across LLM benchmarks (2023–2025).

**Related.** [distillation](distillation.md), [icm-modularity](icm-modularity.md), the [primer](../PRIMER.md).
