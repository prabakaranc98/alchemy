# The Platonic Representation Hypothesis

The Platonic Representation Hypothesis proposes that as models grow more capable their internal representations converge toward a shared, modality-independent statistical model of the world, which different architectures and even different modalities approximate.

Were its strong form correct, transfer would be straightforward: two capable models would already share most of their representational structure, and a thin map between them would suffice. The project does not adopt this assumption, because the strong form is not well supported. Measured convergence is confounded by network scale rather than reflecting a universal tendency, and it is weakest precisely for the narrow, specialised representations the project seeks to move. The defensible position is therefore that the correspondence between two models carries some shared structure but should be assumed partial, particularly for specialties, and that whether a particular capability crosses cleanly is to be established empirically rather than presumed. This is why [independent causal mechanisms](icm-modularity.md) serves as the project's backbone instead.

The evidence is mixed in an instructive way. Representational-similarity measures such as CKA and mutual nearest-neighbour agreement do rise with capability across vision and language models, which supports the hypothesis; but those same measures track scale, and specialised features show the least alignment, which is the regime the project operates in. The practical consequence is to treat any finding of alignment as a measurement at a given scale rather than a guarantee, and to build the transport map as an instrument for measuring alignment per capability rather than assuming it.

**References.** Huh et al., *The Platonic Representation Hypothesis*, `2405.07987`; scale-confound critique, `2602.14486`.

**Related.** [icm-modularity](icm-modularity.md), [transport-ot-gw](transport-ot-gw.md).
