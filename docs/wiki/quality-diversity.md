# Quality-Diversity / Evolutionary Search

**Layer:** Search (outer loop) · **Role:** evolve the transplant recipe instead of hand-designing it

## What it is
**Quality-Diversity (QD)** optimization (e.g. **MAP-Elites**) doesn't seek a single best solution — it fills an **archive** of solutions that are each high-performing *and* diverse along chosen behavior axes. Related population methods: **Evolution Strategies (ES)**, **CycleQD**, **Model Swarms**. Applied here at the **representation level**: evolve *what to transport* — which features, subspaces, steering directions, adapters, merge weights, install layers.

## Why it matters for Alchemy
The pipeline has many knobs (which feature, which map, which layer, which blend). Hand-tuning explores a sliver. **QD searches the recipe space** — and its **diverse-archive objective is a natural match for Alchemy's dual goal: general capacity + many specialties at once** (Open Question 7). It's the **outer loop** wrapping extract→map→install→shape (**Exp 5**).

## The key mechanic
- Define a **behavior descriptor** (e.g. which specialty, diversity, cost) ⇒ a grid of niches.
- Mutate/recombine candidate *recipes*; keep the best per niche ⇒ an archive covering the general+specialty space.
- Quality = the transfer delta; diversity = coverage of capabilities.

## The catch
- **Sample-hungry** — each evaluation is a full transplant+eval; budget carefully.
- **The search-space design does most of the work** — garbage parameterization, garbage archive. So: **use it to search over routes you already have working — not first.** (Discipline: bake-off before search.)

## References
- MAP-Elites (Mouret & Clune, 2015); CycleQD; Model Swarms

## Related
[control-steering](control-steering.md) / [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md) (what it searches over) · [rl-grpo](rl-grpo.md) (gradient alternative) · [model-merging](model-merging.md) (evolving merge recipes)
