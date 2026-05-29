# Glossary

Quick definitions. Linked terms have their own page.

- **Capability** — a behavior/skill a model has (a style, a domain, a reasoning step). Alchemy treats it as a *movable object* when it's modular.
- **Compact knowledge fusion** — the project's framing: capability-level fusion *that downsizes* (flagship → sub-100M). See [knowledge-fusion](knowledge-fusion.md).
- **Crosscoder** — an SAE trained jointly across *two or more* models/layers, yielding shared features usable for transplant. See [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md).
- **Delta (transfer-vs-distillation)** — the headline metric: `score(transfer) − score(plain distillation)` on a held-out eval.
- **Dictionary learning** — decomposing activations into a large set of sparse, interpretable atoms (SAE features).
- **Distillation (KD)** — train a student to match a teacher's outputs/logits. The *baseline*, not the mechanism. See [distillation](distillation.md).
- **Function vector / task vector** — a single vector that, added to activations (function vector) or weights (task vector), induces a behavior. See [control-steering](control-steering.md).
- **Gromov-Wasserstein (GW)** — optimal transport *between different spaces* (compares intra-space distances, not points directly); handles the dimension gap. See [transport-ot-gw](transport-ot-gw.md).
- **ICM (Independent Causal Mechanisms)** — the principle that the world (and good representations) factor into autonomous, reusable modules. The theory backbone. See [icm-modularity](icm-modularity.md).
- **Identifiability** — whether the "true" latent factors can be recovered uniquely. Nonlinear ICA gives conditions. See [nonlinear-ica](nonlinear-ica.md).
- **Install** — the pipeline verb for putting a capability *into* the student so inference needs no teacher.
- **JEPA** — Joint-Embedding Predictive Architecture; train a student to *predict a (frozen) teacher's latents*. See [predictive-jepa](predictive-jepa.md).
- **Modularity** — degree to which a capability is separable from the rest of the network. The project's *binding constraint*.
- **MoE (Mixture of Experts)** — routing tokens to sub-networks; here, a candidate *transfer-ready substrate*.
- **On-policy distillation (OPD)** — student generates rollouts; teacher scores them (dense reverse-KL on student-visited states). Frontier-proven backbone. See [on-policy-distillation](on-policy-distillation.md).
- **Optimal transport (OT)** — cheapest way to move one distribution onto another; gives a coupling/alignment between two spaces. See [transport-ot-gw](transport-ot-gw.md).
- **PRH (Platonic Representation Hypothesis)** — claim that models converge to a shared representation. Strong form contested. See [platonic-representation](platonic-representation.md).
- **Quality-Diversity (QD)** — evolutionary search that fills an *archive* of diverse-yet-good solutions (MAP-Elites). Matches "general + many specialties." See [quality-diversity](quality-diversity.md).
- **Reverse KL** — the KD divergence that's *mode-seeking* (student concentrates on teacher's high-prob modes); can cut diversity. Central to OPD.
- **GRPO** — Group Relative Policy Optimization; a reward-driven RL method. The route that can *exceed* the teacher. See [rl-grpo](rl-grpo.md).
- **SAE (Sparse Autoencoder)** — overcomplete autoencoder with a sparsity penalty; the practical tool for extracting interpretable features. See [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md).
- **Steering vector** — a direction added to the residual stream to push behavior. See [control-steering](control-steering.md).
- **Stitching / affine stitch** — a learned linear (`Wx+b`) map between two models' activation spaces; the cheap baseline *map*.
- **Substrate** — the student architecture, possibly engineered to *receive* transferred capabilities (adapter slots, matched blocks, MoE).
- **Task arithmetic** — adding/subtracting task vectors in weight space to add/remove skills.
- **Transfer-ready** — a student designed up front to make installation easier.
- **Transmutation** — Alchemy's umbrella verb for moving a capability (vs. retrieval/cascades, which are *coupling*, out of scope).
