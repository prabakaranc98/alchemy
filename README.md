# Alchemy — Research Sprint

**Compact knowledge fusion: assembling sub-100M specialists from flagship models, without pretraining.**

*A serious research-and-engineering sprint — survey, build, evaluate, release.*

> **New here?** Read [`docs/PRIMER.md`](docs/PRIMER.md) for the mental model in one sitting, then dive into the [`docs/wiki/`](docs/wiki/) concept pages.

---

## 1. The problem

Pretraining from scratch is expensive, energy-intensive, and out of reach for most researchers — yet flagship models already encode the capabilities a specialist needs.

> **Can we *assemble* a small, specialized model (<100M parameters) out of larger ones — transmuting their capabilities — instead of *training* one from scratch?**

## 2. The central bet

> A specialized sub-100M model can be assembled from larger models by **extracting** a modular capability, **mapping** it across representation spaces, **installing** it into a transfer-ready substrate, and **shaping** it — recovering most of the capability at a fraction of pretraining cost. **The binding constraint is modularity, not compute.**

Governing theory: **independence of causal mechanisms / modularity**. Capabilities that are modular transport cleanly; entangled ones do not. (Sturdier than the Platonic Representation Hypothesis, whose strong form is now contested.)

**Scoping discipline.** The model is the *artifact*; the *characterization* — what transfers, at what cost, under what dimensional gap — is the *contribution*. Success = **transfer-vs-distillation delta** on contamination-resistant evals. We do **not** promise SOTA.

## 3. What to call the work

Verbs in play: *distill, transfer, edit, fuse, transport, transplant*. Literature umbrellas: **model merging** (parameter-space, same-lineage) and **knowledge fusion** (capability-level, cross-architecture — the better fit). Neither implies downsizing. We file under **compact knowledge fusion**; the project is **Alchemy**.

## 4. What we're up against (the honest landscape)

Every lever has a documented failure mode. These are the constraints the sprint must respect, not wish away:

- **Distillation transfers little actual knowledge.** Large fidelity gaps persist even when the student has the capacity to match the teacher — an optimization failure (Stanton et al., NeurIPS 2021). → Treat plain distillation as a *baseline*, not the mechanism.
- **Merging saturates and can't exceed the union of parents.** Interference; a hard ceiling around 4–6 models (`2505.21226`). → Only wins for *combinations* no single model has.
- **Representational convergence is local, not global.** Confounded by network scale; weakest exactly at narrow/specialized representations (Aristotelian rebuttal, `2602.14486`). → Don't assume a specialized capability crosses cleanly. That *is* the experiment.
- **Benchmarks are widely contaminated.** → Measure *deltas* on held-out / self-built evals, never a raw leaderboard.

## 5. The method space

Organized as **layers**, not a flat list of competing tricks. Each layer answers a different question.

### Theory layer — what is movable
- **Independent Causal Mechanisms / modularity** (Parascandolo & Schölkopf, `1712.00961`): mechanisms are autonomous, reusable modules that transfer between problems.
- **Nonlinear ICA / identifiability** (Hyvärinen; iVAE) — the formal bridge between "modular mechanism" and "a real, transportable feature." Sparse autoencoders are the practical, overcomplete descendant.

### Transmutation routes — how a capability moves

| Route | Core move | Best for | Main catch |
|---|---|---|---|
| **Transport** (OT / unbalanced Gromov-Wasserstein) | Align spaces → graft/average | The *map* others reuse; dimension mismatch | Aligns distribution, not function |
| **Dictionary** (crosscoders / SAEs) | Factor into shared features → transplant | Surgical specialty + *verification* of what moved | Only some feature classes transfer |
| **Predictive** (JEPA, frozen-teacher latent prediction) | Student predicts the teacher's latents | **General capacity** | Collapse-prone; you train the student |
| **Control** (steering / function / task vectors, circuit graft) | Extract a direction/circuit → inject | Quick narrow specialty | Shallow, brittle, merge ceiling |

These operate at different points — *extract* (dictionary), *map* (transport), *train-objective* (predictive), *install* (control) — so they **compose** rather than strictly compete.

### Shaping layer — exceeding the source
- **On-policy distillation (OPD).** Student generates its own rollouts; teacher supplies dense reverse-KL supervision on the student-visited states. This is how Qwen3, GLM-5, and MiMo actually transfer capability into smaller models, and it matches full-RL reasoning performance at ~10× lower compute than GRPO. **Multi-Teacher OPD (MOPD)** fuses several domain teachers into one student — compact knowledge fusion, in production.
- **Reward-driven RL (GRPO/PPO-style).** The *only* route that can push the student *beyond* the teacher/union ceiling, via reward extrapolation.
- *Catch:* reverse-KL is mode-seeking (can reduce diversity — watch the "general capacity" goal); RL is sample-hungry, unstable, and capacity-limited at <100M.

### Search layer — optimizing the recipe
- **Representation-level evolutionary / Quality-Diversity** (CycleQD, Model Swarms, ES, MAP-Elites): evolve *what* to transport (features, subspaces, steering directions, adapters, merge recipes). QD's diverse-archive objective is a natural match for the **general capacity + many specialties** dual goal.
- *Catch:* sample-hungry; the search-space design does most of the work. Use it to search over routes you already have — not first.

### Supporting primitives (classical, interdisciplinary)
Not standalone routes — tools that slot under *extract* and *map*:
- **PCA / SVD** — cheap subspace extraction; already used in task-vector merging (SVD on task vectors → low-rank subspaces that reduce interference). A baseline for "what to move."
- **ICA / nonlinear ICA** — the theory of when a direction is an *independent, transportable* source (the modularity ↔ SAE-feature bridge).
- **Manifold learning** (Isomap, diffusion maps, UMAP) — estimates the *intrinsic geometry* that Gromov-Wasserstein transport needs as its ground metric; also diagnostics. (UMAP/t-SNE are non-invertible — diagnostic, not transport.)

### The map underneath
**Optimal transport** (Singh & Jaggi OTFusion, `1910.05653`; unbalanced/fused Gromov-Wasserstein, `2206.09398`) is the connective mapping primitive — it upgrades the affine stitch to an optimal, possibly partial, cross-dimensional coupling, and its cost *operationalizes* "how alignable are these two models."

## 6. The unifying architecture

```
        THEORY:  Independent Causal Mechanisms / modularity (what is movable)
                                  │
  ┌──────────────┬──────────────┬──────────────┬──────────────┐
  │   EXTRACT    │     MAP      │   INSTALL    │    SHAPE      │
  │ crosscoder / │  OT / GW     │  graft /     │  OPD / RL     │
  │ SAE / PCA /  │  (manifold   │  adapt /     │  (exceed the  │
  │ ICA / patch  │   metric)    │  distill /   │   source)     │
  │              │              │  JEPA-predict│               │
  └──────────────┴──────────────┴──────────────┴──────────────┘
                                  │
        SEARCH (outer loop):  representation-level Quality-Diversity / ES
        METRIC:  transfer-vs-distillation delta on contamination-resistant evals
```

## 7. What's good to bet on (recommendation)

- **Proven backbone + baseline-to-beat: on-policy distillation.** It will produce a working specialist cheaply, and it's the floor every novel route must clear. Frontier-validated.
- **The research arm (where the contribution lives): interp-guided transplant — crosscoder *extract* → OT/GW *map* → JEPA-predictive *install*.** The question "does principled transport beat plain OPD, and for which capabilities?" is the paper.
- **Predictive (JEPA)** for the *general-capacity* half; **dictionary + control** for the *specialty* half; **transport** as the map under both; **QD** as the outer-loop search; **RL** when you need to exceed the teacher.
- **Meta-bet:** the *transportability map* — what moves, how, at what cost — is the durable result regardless of which route wins.

## 8. Open questions

1. **Transferability** — Does any specialized capability move into a sub-100M student more faithfully than distillation alone? Is the delta positive?
2. **The map** — How much does a principled transport map (affine → OT → GW) buy over naive matching?
3. **Dimensional gap** — Does a capability survive flagship-dim → <100M-dim under unbalanced GW, and *where* does it break?
4. **Substrate** — Does a transfer-ready architecture (adapter slots, matched blocks, MoE) improve transfer?
5. **Composition** — How many capabilities co-install before interference dominates? Does the 4–6 merge ceiling reappear at the capability level?
6. **Shaping** — Can RL/OPD push the student *beyond* the teacher on the target capability, and does reverse-KL's mode-seeking hurt general capacity?
7. **Search** — Does representation-level QD discover transplant recipes that beat hand-designed ones — and deliver the general+specialty archive?
8. **Theory** — Is transportability *predictable* from a modularity / transport-cost measure?

## 9. The sprint (experiment plan)

Each experiment = one downloadable model + a short writeup.

- **Exp 0 — First commit (the whole game right now).** Smallest closed loop: one narrow capability, a same-family teacher with public SAEs (e.g. Gemma + Gemma Scope), cheapest install, **one** transfer-vs-distillation delta on the board. Establish the OPD baseline here.
- **Exp 1 — Bake-off.** Four end-to-end students for the *same* capability/teacher/eval: OT-fusion, crosscoder-transplant, JEPA-predictive, steering. Plus the OPD baseline. Which route wins for *general* vs *specialty*?
- **Exp 2 — The map.** Swap naive matching for affine stitch → OT → unbalanced GW. Quantify what the principled map buys, especially across the dimensional gap.
- **Exp 3 — Substrate.** Transfer-ready student vs vanilla. Does architecture-for-receiving help?
- **Exp 4 — Shape.** Add OPD/RL on top of the best transplant. Can it exceed the teacher? Track diversity.
- **Exp 5 — Compose & search.** Stack winners (extract → map → install → shape); run representation-level QD to optimize the recipe and build the general+specialty archive. Probe the interference ceiling.

## 10. Strategy & discipline

- **Bake-off first, compose second.** Learn which route wins where, then stack the winners.
- **First commit before first paper.** A downloadable Exp 0 model exists before anything from Exp 1 onward is earned.
- **Max two active threads.** Ship the smallest working loop before scaling. Public within days of functional.
- **Boundary (exclude on purpose):** retrieval / cascades / deferral to the big model are *coupling*, not transmutation — they keep the small model dependent at inference. Out of scope.
- **Honesty:** model = artifact, characterization = contribution. "Most of the capability for a fraction of the cost — and here's exactly where it breaks" beats "I beat the baseline."

## 11. Key references

- Knowledge fusion: *Knowledge Fusion of LLMs* (FuseLLM), `2401.10491` · Model-merging survey (ACM CS 2026), `2408.07666`
- On-policy distillation / RL transfer: Thinking Machines Lab, *On-Policy Distillation* (2025); OPD unification, `2605.16826`; *Learning beyond Teacher* (reward extrapolation), `2602.12125`; Entropy-aware OPD, `2603.07079`
- Optimal transport: OTFusion, `1910.05653`; Fused Unbalanced GW, `2206.09398`
- Predictive / JEPA: SALT (frozen-teacher latent prediction), `2509.24317`; I-JEPA (Assran et al., 2023); US-JEPA, `2602.19322`; Bootleg (multi-layer), `2603.15553`
- Cross-model interpretability: Crosscoders (Lindsey et al., 2024); cross-architecture diffing, `2602.11729`; feature transfer via stitching, `2506.06609`
- Steering / editing: RepE (Zou et al., 2023); function vectors (Todd et al., 2024); task arithmetic (Ilharco et al., 2023)
- Theory: Independent Causal Mechanisms, `1712.00961`; Toward Causal Representation Learning, `2102.11107`; Platonic Representation Hypothesis, `2405.07987`
- Limits: *Does Knowledge Distillation Really Work?*, `2106.05945`; *Why Do More Experts Fail?*, `2505.21226`
