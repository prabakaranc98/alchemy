# Alchemy

**Compact knowledge fusion: assembling sub-100M specialists from flagship models without pretraining.**

Alchemy is a research and engineering programme that surveys, builds, evaluates, and releases small specialised models assembled from larger ones.

For orientation, see [`docs/PRIMER.md`](docs/PRIMER.md); for the individual concepts, the [`docs/wiki/`](docs/wiki/) notes.

## 1. Problem

Pretraining a model from scratch is expensive and out of reach for most researchers, yet flagship models already encode the capabilities a specialist would need. This programme asks whether a small, specialised model of fewer than 100M parameters can be *assembled* from larger ones, by transmuting their capabilities, rather than *trained* from scratch.

## 2. Central hypothesis

A specialised sub-100M model can be assembled from larger models by extracting a modular capability, mapping it across representation spaces, installing it into a transfer-ready substrate, and shaping it, recovering most of the capability at a fraction of the cost of pretraining. The binding constraint is taken to be modularity rather than compute.

The governing theory is the independence of causal mechanisms: capabilities that are modular transport cleanly, while entangled ones do not. This is preferred to the Platonic Representation Hypothesis, whose strong form is now contested. The model produced is the artefact; the characterisation of what transfers, at what cost, and under what dimensional gap is the contribution. Success is measured as the difference between transfer and distillation on contamination-resistant evaluations. The programme does not aim for state-of-the-art results.

## 3. Naming

The relevant operations span several verbs — distil, transfer, edit, fuse, transport, transplant. Two umbrella terms exist in the literature. *Model merging* denotes parameter-space combination of same-lineage models and is therefore weight-space and same-family. *Knowledge fusion*, in the FuseLLM lineage, denotes capability-level combination, is architecture-agnostic, and is framed as the alternative to training from scratch; it is the closer fit. Neither term implies downsizing, and drawing a capability from a flagship into a small specialist is the contribution space. The programme is filed under *compact knowledge fusion* and carries the working name Alchemy.

## 4. Constraints

Each available lever has a documented failure mode, and the programme is designed to respect these rather than to wish them away.

- **Distillation transfers little actual knowledge.** Large fidelity gaps persist even when the student has the capacity to match the teacher, which is a failure of optimisation rather than of capacity (Stanton et al., 2021). Plain distillation is therefore treated as a baseline, not as the mechanism.
- **Merging saturates and cannot exceed the union of its parents.** Interference imposes a practical ceiling at roughly four to six models (`2505.21226`). Gains arise only for combinations no single model possesses.
- **Representational convergence is local rather than global.** It is confounded by network scale and is weakest precisely at the narrow, specialised representations of interest here (`2602.14486`). The correspondence between two models carries some structure, but a specialised capability should not be assumed to cross cleanly; establishing whether it does is the experiment.
- **Benchmarks are widely contaminated.** Differences are therefore measured on held-out or purpose-built evaluations rather than on published leaderboards.

## 5. Method space

The methods are organised as layers, each answering a distinct question, rather than as a flat list of competing techniques.

### Theory: what is movable

The principle of independent causal mechanisms (Parascandolo & Schölkopf, `1712.00961`) holds that mechanisms are autonomous, reusable modules that transfer between problems. Nonlinear ICA and identifiability theory (Hyvärinen; the identifiable VAE) provide the formal bridge between a modular mechanism and a genuine, transportable feature, and sparse autoencoders are their practical, overcomplete descendant.

### Transmutation routes: how a capability moves

| Route | Core operation | Best suited to | Principal limitation |
|---|---|---|---|
| **Transport** (OT, unbalanced Gromov-Wasserstein) | Align spaces, then graft or average | The reusable map; mismatched dimensions | Aligns distribution, not function |
| **Dictionary** (crosscoders, SAEs) | Factor into shared features, then transplant | Surgical specialty, with verification | Only some feature classes transfer |
| **Predictive** (JEPA) | Student predicts the frozen teacher's latents | General capacity | Collapse-prone; the student is trained |
| **Control** (steering) | Extract a direction or circuit, then inject | Quick narrow specialty | Shallow, brittle, merge ceiling |

These act at different points — extraction (dictionary), mapping (transport), training objective (predictive), and installation (control) — and so compose rather than strictly compete.

### Shaping: exceeding the source

On-policy distillation has the student generate its own rollouts while the teacher supplies dense reverse-KL supervision on the student-visited states. It is the means by which several frontier models have been reduced in size, and it reportedly matches full reinforcement-learning reasoning performance at roughly an order of magnitude less compute than GRPO; its multi-teacher variant fuses several domain teachers into one student. Reward-driven reinforcement learning (GRPO or PPO) is the only route able to push the student beyond the teacher or the union of parents, through reward extrapolation. The cautions are that reverse KL is mode-seeking and may reduce diversity, bearing on the general-capacity aim, and that reinforcement learning is sample-hungry, unstable, and limited by the student's capacity at this scale.

### Search: optimising the recipe

Representation-level evolutionary and quality-diversity methods (CycleQD, Model Swarms, evolution strategies, MAP-Elites) search over what to transport — features, subspaces, steering directions, adapters, or merge recipes. The archive objective of quality-diversity suits the dual aim of general capacity alongside several specialties. The method is sample-hungry, and the design of the search space does most of the work; it is therefore best applied to routes already shown to function, rather than first.

### Supporting primitives

These are tools that sit beneath extraction and mapping rather than standalone routes. PCA and SVD provide cheap subspace extraction and are already used in task-vector merging, where SVD yields low-rank subspaces that reduce interference; they form a baseline for what to move. ICA and nonlinear ICA supply the theory of when a direction is an independent, transportable source. Manifold learning (Isomap, diffusion maps, UMAP) estimates the intrinsic geometry that Gromov-Wasserstein transport requires as its ground metric and also serves as a diagnostic, though UMAP and t-SNE are non-invertible and are therefore diagnostic rather than transport tools.

### The underlying map

Optimal transport (Singh & Jaggi, `1910.05653`; unbalanced and fused Gromov-Wasserstein, `2206.09398`) is the connective mapping primitive. It upgrades the affine stitch to an optimal, possibly partial, cross-dimensional coupling, and its cost operationalises how alignable two models are.

## 6. Unifying structure

```
        THEORY:  independent causal mechanisms / modularity (what is movable)
                                  |
  +--------------+--------------+--------------+--------------+
  |   EXTRACT    |     MAP      |   INSTALL    |    SHAPE      |
  | crosscoder / |  OT / GW     |  graft /     |  OPD / RL     |
  | SAE / PCA /  |  (manifold   |  adapt /     |  (exceed the  |
  | ICA / patch  |   metric)    |  distil /    |   source)     |
  |              |              |  JEPA-predict|               |
  +--------------+--------------+--------------+--------------+
                                  |
        SEARCH (outer loop):  representation-level quality-diversity / ES
        METRIC:  transfer-vs-distillation difference on uncontaminated evals
```

## 7. Recommended direction

On-policy distillation serves as the proven backbone and the baseline every novel route must clear; it will produce a working specialist economically and is validated at the frontier. The research contribution lies in interpretation-guided transplant: extraction by crosscoder, mapping by optimal or Gromov-Wasserstein transport, and installation by a predictive objective. The question of whether principled transport improves on plain on-policy distillation, and for which capabilities, is the paper. Within this, the predictive route addresses general capacity, the dictionary and control routes address specialties, transport is the map beneath both, quality-diversity is the outer search, and reinforcement learning is reserved for exceeding the teacher. Whichever route prevails, the durable result is the transportability map — an account of what moves, how, and at what cost.

## 8. Open questions

1. **Transferability.** Does any specialised capability move into a sub-100M student more faithfully than distillation alone, and is the difference reliably positive?
2. **The map.** How much does a principled transport map (affine, then OT, then GW) gain over naive matching?
3. **Dimensional gap.** Does a capability survive the collapse from flagship to sub-100M dimensions under unbalanced GW, and where does it break?
4. **Substrate.** Does a transfer-ready architecture (adapter slots, matched blocks, routing) measurably improve transfer?
5. **Composition.** How many capabilities can be co-installed before interference dominates, and does the four-to-six merge ceiling reappear at the capability level?
6. **Shaping.** Can reinforcement learning or on-policy distillation push the student beyond the teacher on the target capability, and does the mode-seeking of reverse KL harm general capacity?
7. **Search.** Does representation-level quality-diversity discover transplant recipes that improve on hand-designed ones, and does it deliver the general-and-specialty archive?
8. **Theory.** Is transportability predictable from a measure of modularity or transport cost?

## 9. Experiment plan

Each experiment yields one downloadable model and a short written account.

- **Experiment 0 — first commit.** The smallest closed loop: one narrow capability, a same-family teacher with public SAEs (for instance Gemma with Gemma Scope), the cheapest installation, and a single transfer-versus-distillation difference recorded. The on-policy distillation baseline is established here.
- **Experiment 1 — comparison.** Four end-to-end students for the same capability, teacher, and evaluation — OT-fusion, crosscoder-transplant, JEPA-predictive, and steering — alongside the on-policy distillation baseline, to determine which route prevails for general capacity and which for specialty.
- **Experiment 2 — the map.** Replacing naive matching with affine stitch, then OT, then unbalanced GW, to quantify what a principled map provides, particularly across the dimensional gap.
- **Experiment 3 — substrate.** A transfer-ready student against a vanilla one, to test whether architecture designed for receiving helps.
- **Experiment 4 — shaping.** On-policy distillation or reinforcement learning applied on top of the best transplant, to test whether the teacher can be exceeded, while tracking diversity.
- **Experiment 5 — composition and search.** Stacking the winning routes (extract, map, install, shape) and running representation-level quality-diversity to optimise the recipe and build the general-and-specialty archive, while probing the interference ceiling.

## 10. Strategy and discipline

The order of work is comparison first and composition second: establish which route prevails where, then stack the winners. A downloadable Experiment 0 model is expected before anything from Experiment 1 onward is undertaken, so that the first commit precedes the first paper. At most two threads are active at once, the smallest working loop is shipped before scaling, and results are made public within days of becoming functional.

Retrieval, cascades, and deferral to the large model are excluded by design: they are coupling rather than transmutation, and leave the small model dependent at inference. The programme treats the model as the artefact and the characterisation as the contribution; an honest account of how much capability is recovered, at what cost, and where it breaks is preferred to a claim of having beaten a baseline.

## 11. References

- Knowledge fusion: Wan et al., *Knowledge Fusion of LLMs* (FuseLLM), `2401.10491`; model-merging survey (ACM Computing Surveys, 2026), `2408.07666`.
- On-policy distillation and RL transfer: Thinking Machines Lab, *On-Policy Distillation* (2025); OPD unification, `2605.16826`; *Learning beyond Teacher*, `2602.12125`; entropy-aware OPD, `2603.07079`.
- Optimal transport: OTFusion, `1910.05653`; fused unbalanced GW, `2206.09398`.
- Predictive and JEPA: SALT, `2509.24317`; I-JEPA (Assran et al., 2023); US-JEPA, `2602.19322`; Bootleg, `2603.15553`.
- Cross-model interpretability: crosscoders (Lindsey et al., 2024); cross-architecture diffing, `2602.11729`; feature transfer via stitching, `2506.06609`.
- Steering and editing: RepE (Zou et al., 2023); function vectors (Todd et al., 2024); task arithmetic (Ilharco et al., 2023).
- Theory: independent causal mechanisms, `1712.00961`; *Toward Causal Representation Learning*, `2102.11107`; Platonic Representation Hypothesis, `2405.07987`.
- Limits: *Does Knowledge Distillation Really Work?*, `2106.05945`; *Why Do More Experts Fail?*, `2505.21226`.
