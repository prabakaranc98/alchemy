# Reward-driven reinforcement learning

This is reinforcement learning applied to the student against a reward signal, whether verifiable correctness, a reward model, or a preference. Group Relative Policy Optimization is the current workhorse: it samples a group of completions for each prompt, scores them, and updates toward the relatively better ones without a separate value network. Proximal Policy Optimization is the older actor-critic alternative.

Its significance is that every other route is bounded by its source. Distillation cannot exceed the teacher's fidelity, and merging cannot exceed the union of its parents. Reinforcement learning is the one mechanism that can extrapolate beyond these bounds, since a reward allows the student to discover behaviour that no teacher demonstrated. It is therefore the final and optional shaping step, applied on top of the best available transplant.

The update has the student generate, scores the generations by reward, and applies a policy-gradient step toward higher reward; GRPO computes advantages relative to the sampled group rather than from a learned value function, which is cheaper and more stable for language models. In practice it is often layered after [on-policy distillation](on-policy-distillation.md): distil to competence, then optimise to exceed. The cautions are substantial. The method is sample-hungry, unstable, and limited by the student's capacity, so that a sub-100M model may simply be unable to host the extrapolated behaviour; rewards are also susceptible to gaming and must be verifiable or otherwise robust. It is best used to exceed a competent student rather than to train a weak one from the start.

**References.** GRPO (DeepSeekMath and DeepSeek-R1); *Learning beyond Teacher*, `2602.12125`.

**Related.** [on-policy-distillation](on-policy-distillation.md), [model-merging](model-merging.md), [quality-diversity](quality-diversity.md).
