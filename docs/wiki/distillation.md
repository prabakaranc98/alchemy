# Knowledge Distillation (KD)

**Layer:** Framing / baseline · **Role:** the baseline, and a cautionary tale

## What it is
Train a small **student** to match a large **teacher's** outputs — typically the teacher's soft logits ("dark knowledge"), sometimes intermediate features. The classic, off-policy recipe: teacher labels a dataset, student fits those labels.

## Why it matters for Alchemy
KD is the **default thing everyone tries** and therefore the **baseline every Alchemy route must beat.** The whole success metric is defined relative to it: `delta = score(transfer) − score(distillation)`. But it's explicitly **not** the mechanism, because of a hard result:

> **Distillation transfers surprisingly little actual knowledge.** Even when the student *has the capacity* to match the teacher, optimization usually fails to close the fidelity gap — it's an *optimization* failure, not a capacity one (Stanton et al., NeurIPS 2021).

That finding is *why* Alchemy reaches for structural routes ([dictionary](dictionary-sae-crosscoder.md), [transport](transport-ot-gw.md), [JEPA](predictive-jepa.md)) and on-policy shaping ([OPD](on-policy-distillation.md)) instead of trusting plain KD.

## The key mechanic
- Loss = (forward) KL between teacher and student output distributions, often with temperature.
- Off-policy: student trained on teacher-visited (or fixed-dataset) states — *not* its own rollouts. That mismatch is exactly what [OPD](on-policy-distillation.md) fixes.

## The catch
- Fidelity gap persists despite capacity (above).
- Forward KL is mass-covering (student spreads over teacher modes) vs. OPD's mode-seeking reverse KL — different failure modes; know which you're using.

## References
- Hinton et al., *Distilling the Knowledge in a Neural Network* (2015)
- Stanton et al., *Does Knowledge Distillation Really Work?*, `2106.05945`

## Related
[on-policy-distillation](on-policy-distillation.md) (the on-policy fix + backbone) · [predictive-jepa](predictive-jepa.md) (latent-space alternative) · [evals-contamination](evals-contamination.md) (how the delta is measured)
