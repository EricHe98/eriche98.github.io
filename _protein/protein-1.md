---
title: "Markov State Models for Molecular Dynamics"
excerpt: "[Using probabilistic graph models for efficient simulation of the protein folding process](../../files/markov_state_models_for_protein_folding.pdf)"
collection: protein
---

The problem of protein folding is centered around understanding how the chain
of amino acids comprising a protein folds into its stable, low-energy conformation. Because the laws of physics which govern the process are well-defined, protein folding can be simulated on computers. 

However, doing so requires a tremendous amount of computation and generates a large amount of folding
data.

[This paper](../../files/markov_state_models_for_protein_folding.pdf) explains how to build Markov State Models to analyze protein
folding data. As a form of probabilistic graph, Markov State Models can be
used to process folding data generated from molecular dynamics data into a
human-interpretable representation. 

Under the Markov State Model framework,
protein folding is viewed as a probabilistic process rather than a deterministic
one, and uncertainty around estimates can be quantified and used to focus
simulation efforts to reduce the number of computations needed by two orders
of magnitude.