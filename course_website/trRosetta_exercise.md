# Structure Prediction
## Folding algorithm*
Any protein folding algorithm requires:

(1) Search strategy: some way to generate candidate structures (decoys)
(2) Scoring function: discriminate near-native structures from the others

### Fragment-based folding
How does traditional fragment-based folding work in Rosetta? 

1. Generate n-mer fragment library for your given sequence (i.e. for stretches of n residues in your design, find pieces of protein structures from the PDB matching that stretch in sequence and store the corresponding torsion angles in a file)
2. Starting from a fully extended chain, randomly replace the torsion angles for a stretch of n residues with a set from the fragment library file (Monte Carlo sampling w/ Metropolis Criterion)

There are many other details you can read about [here](https://new.rosettacommons.org/docs/latest/application_documentation/structure_prediction/abinitio). sentially this checks to 
\* Adapted from PyRosetta BootCamp 2021
