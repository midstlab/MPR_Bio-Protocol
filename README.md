# Multiply Perturbed Response: A Protocol to Identify Cooperative Allosteric Residue Combinations Driving Protein Conformational Transitions
This repository contains the Jupyter notebook and supporting code used to compute Multiply Perturbed Response (MPR) solutions for a known protein conformational transition (e.g. apo to holo).


## Description
The MPR method identifies single or multiple residues whose collective perturbation best reproduces an experimentally observed conformational change between two protein states.\
\
The workflow supports:
- Structure-based analysis using two PDB files
- Trajectory-based analysis using an MD trajectory (DCD)
- Enumeration and optimization-based solutions for multi-residue perturbations

Jupyter notebook implementing the full MPR workflow, including:
  - Structure superimposition
  - Displacement vector calculation
  - Covariance / inverse Hessian construction
  - MPR overlap (Omax) calculations
  - Output parsing and visualization file generation


## Configuration

Edit the following variables directly in the notebook:\
\
Input files:\
  FINAL_PDB   = "final.pdb"\
  INITIAL_PDB = "initial.pdb"\
  INITIAL_DCD = "initial.dcd"   # optional (trajectory-based analysis)\
\
Perturbation range:\
  Kmin = 1\
  Kmax = 3\
\
Optimization (Gurobi):\
  Optimization-based MPR uses a Gurobi model defined in the notebook. Runtime-related parameters (e.g. time limit, optimality gap) can be adjusted at the point where model parameters are set to shorten computation time for larger systems or higher k values. \
  To use optimization, a Gurobi license should be generated and downloaded.

## Output
results.txt\
  Ranked MPR solutions including:
  - Overlap values (Omax)
  - Residue indices (1-based)
  - Perturbation vectors

bild_files/ \
  ChimeraX-compatible .bild files generated from selected MPR solutions for force-vector visualization.
