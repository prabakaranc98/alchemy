# Predictive route — JEPA (frozen-teacher latent prediction)

**Layer:** Install (route, via training objective) · **Role:** the *general-capacity* spine

## What it is
A **Joint-Embedding Predictive Architecture (JEPA)** trains a model to **predict the latent representation of a target** rather than reconstruct raw inputs or match output logits. In Alchemy's framing: **freeze the flagship as a target encoder; train the tiny student to predict the teacher's latents.** The student learns the teacher's representation *geometry*, not its brittle surface outputs.

## Why it matters for Alchemy
This is the **primary spine for general capacity** and the strongest-evidence install route for the project's goal:

- **Frozen-teacher schemes decouple student and teacher architectures** (SALT) — exactly what you need when the student is a different, much smaller model.
- **Small students do disproportionately well** — compute spent on the *student* (not re-training the teacher) is well spent.
- Predicting *latents* transfers structure that logit-distillation misses (see why plain [distillation](distillation.md) leaks).

It's the **install** step of the headline recipe: *[crosscoder extract](dictionary-sae-crosscoder.md) → [OT/GW map](transport-ot-gw.md) → **JEPA install** → [OPD shape](on-policy-distillation.md).*

## The key mechanic
- Teacher frozen; student + a small **predictor head** trained to match teacher latents at chosen layers (a [mapped](transport-ot-gw.md) target space handles the dimension gap).
- Loss is in *latent* space (cosine/MSE on representations), often asymmetric (predict teacher from student context).
- Multi-layer targets (Bootleg) transfer a richer geometric stack.

## The catch
- **You actually train the student** — more compute than steering/grafting.
- **Representation collapse / brittleness** is the classic JEPA failure (student maps everything to a trivial constant); needs the usual anti-collapse care (stop-grad, asymmetry, variance/covariance terms).

## References
- SALT (frozen-teacher latent prediction), `2509.24317`
- I-JEPA (Assran et al., 2023); US-JEPA, `2602.19322`; Bootleg (multi-layer), `2603.15553`

## Related
[distillation](distillation.md) (what this improves on) · [on-policy-distillation](on-policy-distillation.md) (the shaping step after) · [transport-ot-gw](transport-ot-gw.md) (handles the target-space dimension gap)
