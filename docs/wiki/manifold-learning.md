# Manifold learning

Manifold learning recovers the low-dimensional curved structure on which high-dimensional data lies. Isomap estimates geodesic distances along the data manifold, diffusion maps describe its random-walk geometry, and UMAP and t-SNE produce neighbour-preserving embeddings. Each addresses the question of the true shape and distance structure of a representation.

Its relevance here is that [Gromov-Wasserstein transport](transport-ot-gw.md) requires a ground metric within each space — a measure of how far apart two activations are that respects the representation's actual geometry rather than naive Euclidean distance. Manifold learning supplies that metric: a faithful geometry yields a faithful map, while a poor one distorts the transported capability. The methods also serve as diagnostics, for instance in checking whether a teacher feature and its transplanted counterpart fall in the same neighbourhood.

The procedure builds a neighbour graph, estimates geodesic or diffusion distances or an embedding from it, and feeds the resulting metric to the transport objective as its per-space cost. A caution is essential: UMAP and t-SNE are non-invertible and so are suitable for inspection but not for transport, since a point cannot be mapped back; for the transport metric one should use the geodesic or diffusion distances of Isomap or diffusion maps. Manifold estimates are in any case sensitive to the chosen neighbourhood size and to noise.

**References.** Tenenbaum et al., *Isomap* (2000); Coifman & Lafon, *Diffusion maps* (2006); McInnes et al., *UMAP* (2018).

**Related.** [transport-ot-gw](transport-ot-gw.md), [pca-svd](pca-svd.md).
