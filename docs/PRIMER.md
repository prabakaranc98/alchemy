# Alchemy: an orientation

This note states the problem the project addresses, the assumption it rests on, and the procedure it follows. It is meant to be read once, after which the [wiki](wiki/) pages can be consulted individually.

## The question

Large models already contain the capabilities a small model would need. Alchemy asks whether a specific capability can be moved out of a flagship model and into a sub-100M-parameter specialist, rather than instilled by pretraining the small model from scratch. The working hypothesis is that the obstacle to doing so is representational rather than computational: a capability transfers if it is *modular* in the source model, and resists transfer if it is entangled with the rest of the network. Whether a given capability is modular enough to move is treated as an empirical question, and characterising the answer is the intended contribution.

## Why the question is open

Three results constrain what can be claimed.

First, ordinary distillation transfers less than its premise suggests. Even when a student has the capacity to match a teacher, the optimisation typically fails to close the gap (Stanton et al., 2021). Distillation is therefore a baseline to measure against, not the mechanism to rely on.

Second, parameter-space merging saturates. Combining same-family models cannot exceed the union of their behaviours, and interference imposes a practical ceiling at roughly four to six models. Any genuine gain must come from a combination no single source possesses.

Third, the claim that all capable models converge to a shared representation holds only weakly. Measured convergence is confounded by scale and is least pronounced for the narrow, specialised representations this project cares about. The correspondence between two models carries some structure, but it should be assumed partial rather than complete.

## The procedure

The work decomposes into four operations applied in sequence.

**Extraction** turns a capability into a concrete object — a direction, a feature, a subspace, or a circuit — that can be reasoned about and moved. The principal tools are sparse autoencoders and crosscoders, with PCA and SVD as cheaper baselines (see [dictionary-sae-crosscoder](wiki/dictionary-sae-crosscoder.md), [pca-svd](wiki/pca-svd.md)).

**Mapping** carries that object between two representation spaces that differ in dimension and basis. The methods form a ladder of increasing generality: an affine stitch, optimal transport, and Gromov-Wasserstein transport for the cross-dimensional case (see [transport-ot-gw](wiki/transport-ot-gw.md)).

**Installation** places the capability in the student so that inference no longer depends on the teacher. This is done either by training the student to predict the teacher's latents (see [predictive-jepa](wiki/predictive-jepa.md)) or by editing activations and weights directly (see [control-steering](wiki/control-steering.md)).

**Shaping** improves the installed student, in principle beyond the teacher, through on-policy distillation and reinforcement learning (see [on-policy-distillation](wiki/on-policy-distillation.md), [rl-grpo](wiki/rl-grpo.md)).

An optional outer loop replaces hand-designed recipes with search over them, using quality-diversity methods whose archive structure suits the dual aim of general competence alongside several specialties (see [quality-diversity](wiki/quality-diversity.md)).

## On the four "routes"

The README's route table can read as four competing methods. They are better understood as four emphases within the single procedure above. The dictionary route is extraction taken seriously, and the only path that permits verification of what was moved. The transport route is the mapping primitive the others reuse; on its own it aligns distributions rather than functions. The predictive route is installation by training objective and is the strongest available means of transferring general representational structure. The control route is the cheapest installation and the quickest to a first measurement, at the cost of depth and robustness. The headline research recipe composes them in order: extract with a crosscoder, map with optimal or Gromov-Wasserstein transport, install with a predictive objective, and shape with on-policy distillation.

## What counts as a result

Success is reported as a difference rather than an absolute score: how much a transferred student improves over the same student trained by ordinary distillation, measured on held-out or purpose-built evaluations, since public benchmarks are widely contaminated (see [evals-contamination](wiki/evals-contamination.md)). Because the student carries no teacher at inference, the evaluation must score the standalone model. A null or negative difference is itself informative — evidence that the capability in question is not modular enough to move by the route attempted — and is reported as such. The model is the artefact; the account of what transfers, at what cost, and where it fails is the contribution.

## Reading order

For the theoretical grounding, begin with [icm-modularity](wiki/icm-modularity.md) on why modular structure transfers, followed by [nonlinear-ica](wiki/nonlinear-ica.md) on when an extracted feature is the genuine one. For the first experiment, the relevant pages are [dictionary-sae-crosscoder](wiki/dictionary-sae-crosscoder.md), [control-steering](wiki/control-steering.md), and [on-policy-distillation](wiki/on-policy-distillation.md). For the mapping theory, see [transport-ot-gw](wiki/transport-ot-gw.md). Unfamiliar terms are defined in the [glossary](wiki/glossary.md).
