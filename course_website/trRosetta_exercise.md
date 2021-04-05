# Structure Prediction
## Conceptual overview
Any protein folding algorithm requires:

1. Search strategy: some way to generate candidate structures (decoys)
2. Scoring function: discriminate near-native structures from the others

### Fragment-based folding: AbInitio
How does traditional fragment-based folding work in Rosetta? 

1. For each n-length fragment in your design (sliding window of length n), generate a list of potential backbone geometries that fragment may adopt. Rosetta finds pieces of protein structures from the PDB similar in sequence and stores the corresponding torsion angles typically for 9-/3-mers.
2. Starting from a fully extended chain, randomly replace the torsion angles for a stretch of n residues with a set from the fragment library file (Monte Carlo sampling w/ Metropolis Criterion)

There are many other details you can read about [here](https://new.rosettacommons.org/docs/latest/application_documentation/structure_prediction/abinitio). A design that is likely to fold well into a given structure would result in low RMSD decoys having low energy while high RMSD decoys have high energy (i.e. the desired structure is indeed the lowest energy conformation for the designed sequence). This is commonly referred to as a folding funnel.

![folding_funnel](https://miro.medium.com/max/7334/1*tpZtrx8ZziiTljtQyW4Zmg.png "Folding funnel")

To see a video of ubiquitin folding via fragment-based methods: 

<p align="center">
<a href="https://www.youtube.com/watch?v=TT4syxsh_AU&t=1s" target="_blank"><img src="http://img.youtube.com/vi/TT4syxsh_AU/0.jpg" /></a>
</p>

Some disadvantages of fragment-based methods:

1. Stochastic nature requires generation of a large number of decoys to sample structure space (typically 20k-100k decoys - computationally expensive!) --> the algorithm only works well typically for proteins up to 100 amino acids
2. Success of folding limited by quality of fragments generated. What should we consider when generating fragments? Just sequence? Secondary-structure? Solvent-accessibility? Position in the sequence? What if the protein is dynamic? 

### Machine learning-based folding: trRosetta
How does transform-restrained Rosetta (trRosetta) work? Keep in mind the overall process of modeling. First you want to generate your input features based on domain-knowledge/exploratory data analysis (e.g. perhaps you'd like to transform your data prior to feeding it into your model or use it to generate new features), train your model to minimize a loss function, and use your model to predict outputs (structures) based on new inputs (sequences). 

1. Generate input features. Can be as simple as a single sequence or could include a multi-sequence alignment (coevolutionary coupling features) and/or homologous templates.
2. Feed input through pre-trained deep neural network (trained on non-redundant protein set from PDB)
3. The model will estimate likelihood of inter-residue distances/orientations
4. Probabilities converted into smoothed inter-residue restraints 
5. Generate coarse-grained models via minimization (e.g. gradient descent, BFGS, etc.) in Rosetta
6. Refine with full-atom relax 


![trrosetta](https://yanglab.nankai.edu.cn/trRosetta/help/fig1.png "trRosetta work flow")

This method does not rely on fragments and can also work for de novo designs! The success of this method is dependent on how well the model has learned fundamental sequence-structure relationships.

## Pre-exercise questions:
1. What are some potential properties of a given protein that may make structure prediction difficult? 
2. Would you expect the trRosetta algorithm to perform well on de novo protein designs when it has been trained predominantly on natural proteins?

For the following exercises, before looking at the trRosetta result, take a look at the structure of the protein(s) in question. Skim through the associated primary publication if given to get an idea of how the protein behaves in real life (i.e. not as a static crystal structure). Try to guess what the trRosetta result might be and build your intuition! After doing so, you can check to see how accurate the prediction was by downloading the model and aligning it to the PDB structure in PyMOL. 

All results were generated with MSA + homologous templates unless explicitly stated otherwise.

## Simple case:
Calbindin (PDB ID: 1D1O)
[trRosetta result](https://yanglab.nankai.edu.cn/trRosetta/output/TR040039/)
- Monomeric, well-packed, many homologous proteins available 

## Difficult cases:
1. Two topologies separated by a point mutation [source](https://www.pnas.org/content/106/50/21149)
- IgG-binding (PDB ID: 2LHC) [trRosetta result](https://yanglab.nankai.edu.cn/trRosetta/output/TR035112/)
- Albumin-binding (PDB ID: 2LHD) [MSA + templates result](https://yanglab.nankai.edu.cn/trRosetta/output/TR035114/), [single sequence + templates result](https://yanglab.nankai.edu.cn/trRosetta/output/TR040173/), [single sequence + no templates result](https://yanglab.nankai.edu.cn/trRosetta/output/TR040175/)

2. Fold switch XCL1 (PDB ID: 2JP1, 1J9O) [source](https://pubs.acs.org/doi/10.1021/acschembio.8b00276), [trRosetta result](https://yanglab.nankai.edu.cn/trRosetta/output/TR035121/)

3. Ideal de novo + redesigned de novo fold

## Comparing to AlphaFold2:
## Post-exercise questions:

\* Adapted from PyRosetta BootCamp 2021
