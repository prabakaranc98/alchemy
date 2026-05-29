# Quality-diversity and evolutionary search

Quality-diversity optimisation, of which MAP-Elites is the canonical instance, does not seek a single best solution but maintains an archive of solutions that are each high-performing and mutually diverse along chosen behavioural axes. Related population-based methods include evolution strategies, CycleQD, and Model Swarms. Applied at the level of representations, the search ranges over what to transport: which features, subspaces, steering directions, adapters, or merge coefficients.

The procedure has many degrees of freedom — which feature, which map, which layer, which blend — and hand-tuning explores only a small part of that space. Quality-diversity search addresses the recipe space directly, and its archive objective suits the project's twofold aim of general competence alongside several specialties, since the archive is designed to cover a range of behaviours rather than to optimise one. It is the outer loop enclosing the extract-map-install-shape sequence.

In operation a behaviour descriptor — which specialty, what diversity, what cost — defines a set of niches; candidate recipes are mutated and recombined, and the best in each niche is retained, so that the archive comes to span the general and specialty space, with quality given by the transfer difference and diversity by the coverage of capabilities. Two cautions apply. Each evaluation is a full transplant and assessment, so the method is sample-hungry and its budget must be managed. And the design of the search space does most of the work, since a poor parameterisation yields a poor archive; the method is therefore best applied to search over routes already shown to work, after the comparative study rather than before it.

**References.** Mouret & Clune, *MAP-Elites* (2015); CycleQD; Model Swarms.

**Related.** [control-steering](control-steering.md), [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md), [rl-grpo](rl-grpo.md), [model-merging](model-merging.md).
