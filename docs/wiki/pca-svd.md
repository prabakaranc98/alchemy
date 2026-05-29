# Principal components and singular value decomposition

Principal component analysis identifies the orthogonal directions of greatest variance in a set of activations; the singular value decomposition factorises a matrix, such as a weight difference or a task vector, into low-rank components. Both yield a subspace — a compact basis for the part of the representation where a capability is concentrated.

Before the heavier interpretable machinery is invoked, these provide the baseline answer to the question of where in the model a capability resides. The approach is established in merging, where applying the singular value decomposition to task vectors yields low-rank subspaces that reduce interference when models are combined; this bears directly on the [merging ceiling](model-merging.md) and on the composition question. If a capability is well captured by a few principal directions, the more elaborate extraction may be unnecessary, and the linear subspace serves as the null hypothesis against which the [dictionary route](dictionary-sae-crosscoder.md) must justify itself.

In practice one collects activations or a weight difference, centres them, and retains the leading singular vectors to obtain a low-dimensional subspace within which transplant or merging is performed, reducing collision between tasks relative to working in the full space. The limitation is that these directions maximise variance rather than independence or interpretability and therefore mix mechanisms together, which is precisely the gap that [independent component analysis](nonlinear-ica.md) addresses; and being linear, they miss curved structure that [manifold methods](manifold-learning.md) recover.

**References.** Task-vector and low-rank merging, see [model-merging](model-merging.md).

**Related.** [dictionary-sae-crosscoder](dictionary-sae-crosscoder.md), [nonlinear-ica](nonlinear-ica.md), [model-merging](model-merging.md).
