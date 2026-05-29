# PCA / SVD — subspace extraction

**Layer:** Extract (supporting primitive) · **Role:** the cheap "what to move" baseline

## What it is
**PCA** finds the orthogonal directions of maximum variance in activations; **SVD** factorizes a matrix (e.g. a weight delta or a task vector) into low-rank components. Both give you a **subspace** — a compact basis for "where the action is."

## Why it matters for Alchemy
Before reaching for SAEs or crosscoders, PCA/SVD is the **baseline answer to "what part of the model carries this capability?"** It's already proven in merging: running **SVD on task vectors** yields low-rank subspaces that **reduce interference** when combining models — directly relevant to the [merging ceiling](model-merging.md) and to [composition](quality-diversity.md). If a capability is well-captured by a few principal directions, you may not need the heavier interpretable machinery at all.

## The key mechanic
- Collect activations (or a weight/task-vector delta), center, take top-k singular vectors ⇒ a k-dim subspace.
- Transplant/merge within that subspace instead of full-dim ⇒ less cross-task collision.
- Serves as the **null hypothesis** for the [dictionary route](dictionary-sae-crosscoder.md): "does sparse, interpretable extraction beat plain low-rank?"

## The catch
- PCA directions are **variance-maximizing, not independent or interpretable** — they mix mechanisms (this is exactly the gap [ICA/nonlinear-ICA](nonlinear-ica.md) addresses).
- Linear only; misses curved structure that [manifold methods](manifold-learning.md) capture.

## References
- Task-vector + SVD low-rank merging (task arithmetic line, see [model-merging](model-merging.md))

## Related
[dictionary-sae-crosscoder](dictionary-sae-crosscoder.md) (the interpretable upgrade) · [nonlinear-ica](nonlinear-ica.md) (independence vs. variance) · [model-merging](model-merging.md) (where SVD subspaces reduce interference)
