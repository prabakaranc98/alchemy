# Control route — Steering / Function / Task Vectors

**Layer:** Install (route) · **Role:** the cheapest install — fast first delta

## What it is
A family of **activation/weight-editing** techniques:
- **Steering vectors** (RepE): add a direction to the residual stream at inference to push behavior.
- **Function vectors** (Todd et al.): a single vector extracted from in-context examples that *induces a task* when added to activations.
- **Task vectors / task arithmetic** (Ilharco et al.): `θ_finetuned − θ_base` is a weight-space direction you can add/subtract to add/remove a skill.
- **Circuit grafting**: copy a small subgraph of weights/heads that implement a behavior.

## Why it matters for Alchemy
This is the **cheapest install verb** and the fastest route to **one delta on the board** for a narrow specialty — ideal for **Exp 0**. Workflow: [extract](dictionary-sae-crosscoder.md) a direction (e.g. a Gemma Scope feature) → [map](transport-ot-gw.md) it to student space → add it as a steering vector → **fold it into the weights** so inference needs no teacher (staying inside the "no coupling" boundary). It's also one of the two routes (with the [dictionary](dictionary-sae-crosscoder.md)) where you can *verify* what moved.

## The key mechanic
- Find a direction (difference-of-means between contrastive prompts, an SAE decoder column, or a fit function vector).
- Inject: `h ← h + α·d` at a chosen layer; sweep `α`.
- Persist: fold the steer into the layer's bias/weights ⇒ standalone artifact.

## The catch
- **Shallow and brittle** — one direction rarely captures a rich capability; effects are layer- and scale-sensitive.
- **Merge ceiling**: stacking many task/steering vectors hits interference fast (the [4–6 model ceiling](model-merging.md) at the capability level — Open Question 5).
- Great as a baseline and a first loop; not the route that carries *general* capacity (that's [JEPA](predictive-jepa.md)).

## References
- RepE (Zou et al., 2023); Function vectors (Todd et al., 2024); Task arithmetic (Ilharco et al., 2023)

## Related
[dictionary-sae-crosscoder](dictionary-sae-crosscoder.md) (where the direction comes from) · [model-merging](model-merging.md) (the interference ceiling) · [predictive-jepa](predictive-jepa.md) (the deeper install for general capacity)
