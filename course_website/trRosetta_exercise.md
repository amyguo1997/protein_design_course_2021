# Structure Prediction
Any protein folding algorithm requires:

1. Search strategy: some way to generate candidate structures (decoys)
2. Scoring function: discriminate near-native structures from the others

### Fragment-based folding: AbInitio
How does traditional fragment-based folding work in Rosetta? 

1. For each n-length fragment in your design (sliding window of length n), generate a list of potential backbone geometries that fragment may adopt. Rosetta finds pieces of protein structures from the PDB similar in sequence and stores the corresponding torsion angles typically for 9-/3-mers.
2. Starting from a fully extended chain, randomly replace the torsion angles for a stretch of n residues with a set from the fragment library file (Monte Carlo sampling w/ Metropolis Criterion)

There are many other details you can read about [here](https://new.rosettacommons.org/docs/latest/application_documentation/structure_prediction/abinitio). A design that is likely to fold well into a given structure would result in low RMSD decoys having low energy while high RMSD decoys have high energy (i.e. the desired structure is indeed the lowest energy conformation for the designed sequence). This is commonly referred to as a folding funnel.

<p align="center">
![folding_funnel](https://miro.medium.com/max/7334/1*tpZtrx8ZziiTljtQyW4Zmg.png "Folding funnel")
</p>

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

<p align="center">
![trrosetta](https://yanglab.nankai.edu.cn/trRosetta/help/fig1.png "trRosetta work flow")
</p>

\* Adapted from PyRosetta BootCamp 2021
