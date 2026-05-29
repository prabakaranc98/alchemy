# Alchemy Primer

*The whole mental model in one sitting. Read top to bottom once; after that, jump straight to the [wiki](wiki/) page you need.*

---

## The one-sentence version

Big models already contain the capabilities a small model needs; **Alchemy tries to move a capability out of a flagship and into a sub-100M specialist** — extract it, map it across the representation gap, install it, and shape it — instead of paying to pretrain from scratch. The bet is that **the limiting factor is whether the capability is *modular*, not how much compute you have.**

## Why this could work (and why it might not)

A capability lives somewhere in a network's activations. If it's a clean, separable *module* (think: "this direction = formal tone," "this circuit = arithmetic carry"), you should be able to lift it out and graft it elsewhere. If it's smeared across the whole network and entangled with everything else, no amount of clever mapping will pull it loose. **Which capabilities are modular enough to move is an empirical question — and answering it *is* the research.**

Three hard facts keep the project honest:
1. **Plain distillation leaks.** Even when a small student *could* match a teacher, the optimization usually doesn't get there ([distillation](wiki/distillation.md)). So distillation is the *baseline to beat*, not the trick.
2. **Merging has a ceiling.** Combining same-family models saturates around 4–6 and can't exceed their union ([model merging](wiki/model-merging.md)). Real wins must come from *combinations no single parent has*.
3. **"All models converge to one representation" is overstated.** Convergence is strongest for generic features and *weakest exactly for the narrow, specialized ones we care about* ([Platonic Representation](wiki/platonic-representation.md)). The bridge between two models carries *some* structure — assume it's partial.

## The pipeline: four verbs

Everything in Alchemy is one of four moves. This is the spine — memorize it.

```
  EXTRACT  ──▶  MAP  ──▶  INSTALL  ──▶  SHAPE
 (find it)  (carry it) (graft it)  (improve it)
```

| Verb | Question | Main tools | Wiki |
|---|---|---|---|
| **Extract** | *What* is the capability, as a concrete object (direction / feature / subspace / circuit)? | SAEs, crosscoders, PCA/SVD, ICA, activation patching | [dictionary](wiki/dictionary-sae-crosscoder.md), [pca-svd](wiki/pca-svd.md), [nonlinear-ica](wiki/nonlinear-ica.md) |
| **Map** | How do I carry that object across two *different* representation spaces (different dim, different basis)? | affine stitch → optimal transport → Gromov-Wasserstein | [transport](wiki/transport-ot-gw.md), [manifold-learning](wiki/manifold-learning.md) |
| **Install** | How do I put it *into* the student so inference needs no teacher? | weight graft, adapters, JEPA latent-prediction training, distillation | [predictive/JEPA](wiki/predictive-jepa.md), [control/steering](wiki/control-steering.md) |
| **Shape** | How do I make the student *better* — even beyond the teacher? | on-policy distillation, RL (GRPO) | [on-policy-distillation](wiki/on-policy-distillation.md), [rl-grpo](wiki/rl-grpo.md) |

And wrapping the whole thing, an optional **outer loop**:

- **Search** — instead of hand-designing the recipe, *evolve* it: which features, which subspaces, which install layer, which merge weights. Quality-Diversity is the natural fit because we want *general capacity + many specialties* at once ([quality-diversity](wiki/quality-diversity.md)).

## The four "routes" are just different paths through the pipeline

The README's route table can look like four rival methods. They're not rivals — **they emphasize different verbs and compose.**

- **Dictionary route** = strong **extract** (factor into interpretable features, transplant the one you want). The only route that lets you *verify what moved*. Best for narrow specialties.
- **Transport route** = strong **map** (optimal coupling between spaces). It's the connective tissue the others reuse, especially across the dimension gap. By itself it aligns *distribution*, not *function* — so it's a primitive, not a hero.
- **Predictive route (JEPA)** = strong **install** via training objective (freeze teacher, train student to predict teacher's latents). Transfers representation *geometry* rather than brittle outputs. Best for *general capacity* — this is the primary spine.
- **Control route (steering)** = cheapest **install** (add a direction at inference, then fold into weights). Fast and verifiable but shallow and brittle. Great for a first delta.

The headline research recipe stacks them: **crosscoder extracts → OT/GW maps → JEPA-objective installs → OPD/RL shapes.**

## What "success" means here

Not a leaderboard number. The deliverable is a **delta**: how much better the transferred student is than the *same student trained with plain distillation*, measured on a **held-out / self-built eval** (because public benchmarks are contaminated — [evals-contamination](wiki/evals-contamination.md)).

> **The model is the artifact. The *characterization* — what transfers, at what cost, where it breaks — is the contribution.**

A negative or zero delta is still a result ("this capability isn't modular enough to move that way"). That honesty is the point.

## How to read the rest of the docs

- **Want the theory grounding?** → [icm-modularity](wiki/icm-modularity.md) (why modular things transfer), then [nonlinear-ica](wiki/nonlinear-ica.md) (when a "feature" is real and movable).
- **Want to build Exp 0?** → [dictionary](wiki/dictionary-sae-crosscoder.md) + [control/steering](wiki/control-steering.md) + [on-policy-distillation](wiki/on-policy-distillation.md) (the baseline).
- **Want the map math?** → [transport-ot-gw](wiki/transport-ot-gw.md).
- **Lost on a term?** → [glossary](wiki/glossary.md).

## The 30-second recall

> Capabilities are (sometimes) modules. **Extract** the module, **map** it across the gap, **install** it teacher-free, **shape** it past the baseline. The baseline is on-policy distillation; the contribution is the *map of what moves and what doesn't*. Modularity is the constraint, not compute.
