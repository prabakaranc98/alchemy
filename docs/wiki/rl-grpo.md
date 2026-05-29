# Reward-driven RL (GRPO / PPO-style)

**Layer:** Shape · **Role:** the *only* route that can push the student **beyond** the teacher

## What it is
Reinforcement learning on the student against a **reward signal** (verifiable correctness, a reward model, a preference). **GRPO (Group Relative Policy Optimization)** is the current workhorse: sample a group of completions per prompt, score them, and push toward the relatively-better ones (no separate value network). PPO is the older actor-critic alternative.

## Why it matters for Alchemy
Every other route is bounded by its source: distillation and merging **can't exceed the teacher / union of parents** (the [merging ceiling](model-merging.md), the [distillation](distillation.md) fidelity gap). **RL is the one lever that can extrapolate past it** — via reward, the student can discover behavior neither teacher demonstrated (Open Question 6, "Learning beyond Teacher"). It's the final, optional **shape** step on top of the best transplant (**Exp 4**).

## The key mechanic
- Student generates ⇒ reward scores ⇒ policy-gradient update toward higher reward.
- GRPO uses *group-relative* advantages (rank within a sampled batch) instead of a learned value function — cheaper, stabler for LLMs.
- Often layered *after* [OPD](on-policy-distillation.md): distill to competence, then RL to exceed.

## The catch
- **Sample-hungry, unstable, and capacity-limited at <100M** — a tiny student may simply lack the capacity to host the extrapolated behavior.
- Reward hacking; needs verifiable or robust rewards.
- Use it to *exceed*, not to *bootstrap* — it's expensive to drive a weak student from scratch.

## References
- GRPO (DeepSeekMath / DeepSeek-R1 line)
- *Learning beyond Teacher* (reward extrapolation), `2602.12125`

## Related
[on-policy-distillation](on-policy-distillation.md) (the cheaper backbone; RL goes on top) · [model-merging](model-merging.md) (the ceiling RL is meant to break) · [quality-diversity](quality-diversity.md) (population-based alternative/complement)
