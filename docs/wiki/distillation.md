# Knowledge distillation

Knowledge distillation trains a small student to reproduce the outputs of a large teacher, typically the teacher's soft logits and sometimes its intermediate features. In the classical, off-policy form the teacher labels a dataset and the student is fitted to those labels.

It is the default method and therefore the baseline against which the project's routes are measured; the success criterion is defined relative to it, as the difference in score between the transferred student and the same student trained by distillation. It is deliberately not the mechanism the project relies on, for a documented reason: distillation transfers less than its premise implies. Even when the student has the capacity to match the teacher, the optimisation generally fails to close the fidelity gap, which is a failure of optimisation rather than of capacity (Stanton et al., 2021). This finding is the reason the project turns to the structural routes — [dictionary](dictionary-sae-crosscoder.md), [transport](transport-ot-gw.md), and [predictive](predictive-jepa.md) — and to on-policy shaping rather than relying on distillation alone.

The loss is the forward KL divergence between teacher and student output distributions, usually with a temperature; the off-policy character means the student is trained on teacher-visited or fixed states rather than its own rollouts, and that mismatch is what [on-policy distillation](on-policy-distillation.md) corrects. Two points are worth keeping distinct: the fidelity gap persists despite adequate capacity, and forward KL is mass-covering, spreading the student over the teacher's modes, in contrast to the mode-seeking reverse KL of on-policy distillation — different methods with different failure modes.

**References.** Hinton et al., *Distilling the Knowledge in a Neural Network* (2015); Stanton et al., *Does Knowledge Distillation Really Work?*, `2106.05945`.

**Related.** [on-policy-distillation](on-policy-distillation.md), [predictive-jepa](predictive-jepa.md), [evals-contamination](evals-contamination.md).
