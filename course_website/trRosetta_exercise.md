# Structure Prediction
## Folding algorithm*
Any protein folding algorithm requires:

1. Search strategy: some way to generate candidate structures (decoys)
2. Scoring function: discriminate near-native structures from the others

### Fragment-based folding
How does traditional fragment-based folding work in Rosetta? 

1. For each n-length fragment in your design (sliding window of length n), generate a list of potential backbone geometries that fragment may adopt. Rosetta finds pieces of protein structures from the PDB similar in sequence and stores the corresponding torsion angles typically for 9-/3-mers.
2. Starting from a fully extended chain, randomly replace the torsion angles for a stretch of n residues with a set from the fragment library file (Monte Carlo sampling w/ Metropolis Criterion)

There are many other details you can read about [here](https://new.rosettacommons.org/docs/latest/application_documentation/structure_prediction/abinitio). A design that is likely to fold well into a given structure would result in low RMSD decoys having low energy while high RMSD decoys have high energy (i.e. the desired structure is indeed the lowest energy conformation for the designed sequence). This is commonly referred to as a folding funnel.

To see a video of ubiquitin folding via fragment-based methods: 
<a href="https://www.youtube.com/watch?v=TT4syxsh_AU&t=1s" target="_blank"><img src="http://img.youtube.com/vi/TT4syxsh_AU/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

What are some disadvantages of fragment-based methods? 

1. Stochastic nature requires generation of a large number of decoys to sample structure space (typically 20k-100k decoys - computationally expensive!) --> the algorithm only works well typically for proteins up to 100 amino acids
2. Success of folding limited by quality of fragments generated. What should we consider when generating fragments? Just sequence? Secondary-structure? Solvent-accessibility? Position in the sequence? What if the protein is dynamic? 

\* Adapted from PyRosetta BootCamp 2021
