# Manifold Learning

**Layer:** Map (supporting primitive) · **Role:** estimate the intrinsic geometry transport needs

## What it is
Methods that recover the **low-dimensional curved structure** (manifold) that high-dimensional data lies on: **Isomap** (geodesic distances), **diffusion maps** (random-walk geometry), **UMAP / t-SNE** (neighbor-preserving embeddings). They answer "what's the *true* shape and distance structure of this representation?"

## Why it matters for Alchemy
[Gromov-Wasserstein transport](transport-ot-gw.md) needs a **ground metric within each space** — a notion of "how far apart are these two activations" that respects the representation's real geometry, not naive Euclidean distance. Manifold learning **supplies that metric**. Get the geometry right and GW maps faithfully; get it wrong and the map distorts the capability.

They're also **diagnostics**: visualize whether a teacher feature and its transplanted student counterpart actually land in the same neighborhood.

## The key mechanic
- Build a neighbor graph ⇒ estimate geodesic/diffusion distances ⇒ a metric (Isomap, diffusion maps) or an embedding (UMAP).
- Feed the *metric* into GW as its per-space cost matrix.

## The catch
- **UMAP and t-SNE are non-invertible** — great for *looking*, useless for *transporting* (you can't map a point back). Use them as diagnostics only; use Isomap/diffusion-map *distances* for the GW metric.
- Manifold estimates are sensitive to neighbor count and noise.

## References
- Isomap (Tenenbaum et al., 2000); Diffusion maps (Coifman & Lafon, 2006); UMAP (McInnes et al., 2018)

## Related
[transport-ot-gw](transport-ot-gw.md) (consumes the metric) · [pca-svd](pca-svd.md) (the linear-geometry baseline)
