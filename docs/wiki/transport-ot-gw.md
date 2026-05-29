# Transport route — Optimal Transport & Gromov-Wasserstein

**Layer:** Map (route + the primitive underneath) · **Role:** carry an object across two different spaces

## What it is
**Optimal Transport (OT)** finds the cheapest way to move one probability distribution onto another, yielding a **coupling** — a soft correspondence between points in space A and space B. **OTFusion** uses this to align two networks' neurons before averaging. **Gromov-Wasserstein (GW)** generalizes OT to **spaces that aren't directly comparable** (different dimensions, no shared coordinates): instead of comparing points, it compares *intra-space distance structures*. **Unbalanced / fused GW** further allows partial matching (not all mass must move) and incorporating feature similarity.

## Why it matters for Alchemy
This is the **map** verb — the connective tissue every route reuses, and the answer to the **flagship-dim → sub-100M-dim gap**. The progression is the project's actual research ladder (Open Question 2):

> **affine stitch → OT → unbalanced GW**

- An **affine stitch** (`Wx+b`, least-squares) is the cheap baseline map (good enough for Exp 0).
- **OT** gives an optimal, distribution-aware coupling.
- **GW** is what you need when the two spaces have *different dimensions* — exactly the downsizing case.

Crucially, **the transport *cost* operationalizes "how alignable are these two models"** — it's a candidate modularity/transferability measure (Open Question 8), not just a tool.

## The key mechanic
- OT: minimize total moving cost given a ground metric between points ⇒ coupling matrix.
- GW: minimize distortion of *pairwise distances* between spaces ⇒ works with no shared coordinate system. Needs a **ground metric per space** — which is where [manifold learning](manifold-learning.md) supplies intrinsic geometry.
- Output is a map you apply to the [extracted](dictionary-sae-crosscoder.md) feature/subspace before [installing](control-steering.md) it.

## The catch
- **OT aligns *distribution*, not *function*** — matched neurons can have aligned statistics but different computational roles. Alignment ≠ capability transfer.
- GW is non-convex and costly; partial/unbalanced variants add hyperparameters.
- A primitive, **not a standalone hero**: it maps, it doesn't install or shape.

## References
- Singh & Jaggi, *Model Fusion via Optimal Transport (OTFusion)*, `1910.05653`
- *Fused Unbalanced Gromov-Wasserstein (FUGW)*, `2206.09398`

## Related
[manifold-learning](manifold-learning.md) (the ground metric GW needs) · [pca-svd](pca-svd.md) (what gets mapped) · [platonic-representation](platonic-representation.md) (transport cost *measures* the alignment PRH assumes) · [control-steering](control-steering.md) (installing the mapped object)
