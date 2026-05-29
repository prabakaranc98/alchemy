# Glossary

Definitions of terms used across the wiki. Cross-referenced terms have their own note.

**Capability.** A skill or behaviour a model exhibits, such as a register, a domain competence, or a reasoning step. The project treats it as a movable object when it is modular.

**Compact knowledge fusion.** The project's framing: capability-level fusion that also reduces scale, from a flagship to a sub-100M specialist. See [knowledge-fusion](knowledge-fusion.md).

**Crosscoder.** A sparse autoencoder trained jointly across two or more models or layers, yielding features in a shared space suitable for transplant. See [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md).

**Difference (transfer versus distillation).** The project's headline metric: the score of the transferred student minus that of the distillation baseline, on a held-out evaluation.

**Dictionary learning.** The decomposition of activations into a large set of sparse, interpretable atoms, as performed by a sparse autoencoder.

**Distillation.** Training a student to reproduce a teacher's outputs. The project treats it as a baseline rather than a mechanism. See [distillation](distillation.md).

**Function vector, task vector.** A vector that induces a behaviour when added to activations (function vector) or to weights (task vector). See [control-steering](control-steering.md).

**Gromov-Wasserstein.** Optimal transport between spaces that are not directly comparable, which matches intra-space distances rather than points and so handles a difference in dimension. See [transport-ot-gw](transport-ot-gw.md).

**GRPO.** Group Relative Policy Optimization, a reward-driven reinforcement-learning method, and the route that can carry a student beyond its teacher. See [rl-grpo](rl-grpo.md).

**Independent causal mechanisms.** The principle that a process factorises into autonomous, reusable modules; the project's theoretical backbone. See [icm-modularity](icm-modularity.md).

**Identifiability.** Whether the true latent factors of a representation can be recovered uniquely; nonlinear ICA gives conditions under which they can. See [nonlinear-ica](nonlinear-ica.md).

**Installation.** The operation of placing a capability into the student so that inference no longer requires the teacher.

**JEPA.** A joint-embedding predictive architecture; here, training a student to predict a frozen teacher's latents. See [predictive-jepa](predictive-jepa.md).

**Modularity.** The degree to which a capability is separable from the rest of the network; the project's binding constraint.

**Mixture of experts.** An architecture that routes inputs to specialised sub-networks; considered here as a candidate transfer-ready substrate.

**On-policy distillation.** Distillation in which the student generates its own rollouts and the teacher supplies dense reverse-KL supervision on the student-visited states. See [on-policy-distillation](on-policy-distillation.md).

**Optimal transport.** The least-cost mapping of one distribution onto another, yielding a coupling between two spaces. See [transport-ot-gw](transport-ot-gw.md).

**Platonic Representation Hypothesis.** The claim that capable models converge to a shared representation; its strong form is not well supported. See [platonic-representation](platonic-representation.md).

**Quality-diversity.** Evolutionary search that maintains an archive of diverse, high-performing solutions; suited to the aim of general competence with several specialties. See [quality-diversity](quality-diversity.md).

**Reverse KL.** The divergence used in on-policy distillation; it is mode-seeking, concentrating the student on the teacher's dominant modes, and can reduce diversity.

**Sparse autoencoder.** An overcomplete autoencoder with a sparsity penalty, the practical tool for extracting interpretable features. See [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md).

**Steering vector.** A direction added to the residual stream to shift behaviour. See [control-steering](control-steering.md).

**Stitching, affine stitch.** A learned linear map between two models' activation spaces; the cheap baseline for the mapping operation.

**Substrate.** The student's architecture, possibly designed in advance to receive transferred capabilities through adapter slots, matched blocks, or routing.

**Task arithmetic.** The addition and subtraction of task vectors in weight space to introduce or remove skills.

**Transfer-ready.** Of a student, designed from the outset to make installation easier.

**Transmutation.** The project's umbrella term for moving a capability, as distinct from retrieval or cascades, which leave the small model dependent on the large one and are out of scope.
