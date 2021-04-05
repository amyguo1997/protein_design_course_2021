# Structure Prediction
## Folding algorithm*
Any protein folding algorithm requires:

(1) Search strategy: some way to generate candidate structures (decoys)
(2) Scoring function: discriminate near-native structures from the others

### Fragment-based folding
How does traditional fragment-based folding work in Rosetta? 

1. Generate n-mer fragment library for your given sequence (i.e. for stretches of n residues in your design, find pieces of protein structures from the PDB matching that stretch in sequence and store the corresponding torsion angles in a file)
2. Starting from a fully extended chain, randomly replace the torsion angles for a stretch of n residues with a set from the fragment library file (Monte Carlo sampling w/ Metropolis Criterion)

There are many other details you can read about [here](https://new.rosettacommons.org/docs/latest/application_documentation/structure_prediction/abinitio). A design that is likely to fold well into a given structure would result in low RMSD decoys having low energy while high RMSD decoys have high energy (i.e. the desired structure is indeed the lowest energy conformation for the designed sequence). This is commonly referred to as a folding funnel.

To see a video of ubiquitin folding via fragment-based methods: 
<a href="https://www.youtube.com/watch?v=TT4syxsh_AU&t=1s" target="_blank"><img src="http://img.youtube.com/vi/TT4syxsh_AU&t/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>
\* Adapted from PyRosetta BootCamp 2021
