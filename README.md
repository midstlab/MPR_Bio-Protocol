# Multiply Perturbed Response: A Computational Protocol to Identify Cooperative Allosteric Residue Combinations Driving Protein Conformational Transitions

This repository contains the Jupyter notebook and supporting files used to compute Multiply Perturbed Response (MPR) solutions for a known protein conformational transition.

The example dataset provided in this repository uses Ferric Binding Protein (FBP) as a test system. The initial and final structures represent the apo and holo states of FBP, respectively, and the optional trajectory-based example uses a 40-ns equilibrated plateau segment extracted from a 100-ns MD simulation of the initial apo state.

## Description

The MPR method identifies single or multiple residues whose collective perturbation best reproduces an experimentally observed or computationally defined conformational change between two protein states.

The workflow supports:

- Structure-based analysis using two PDB files
- Trajectory-based analysis using an MD trajectory in DCD format
- Enumeration and optimization-based solutions for multi-residue perturbations
- ChimeraX-compatible visualization of optimized force vectors

The Jupyter notebook implements the full MPR workflow, including:

- Structure superimposition
- Displacement vector calculation
- Covariance or inverse Hessian construction
- MPR overlap (Omax) calculations
- Output parsing
- Visualization file generation

## Repository contents

- `MPR.ipynb`: Main Jupyter notebook implementing the full MPR workflow.
- `environment.yml`: Conda environment file containing the recommended Python 3.8 environment and required packages for the ProDy-based workflow.
- `initial.pdb`: Initial-state structure used in the example analysis.
- `final.pdb`: Final-state structure used in the example analysis.
- `initial.dcd`: Optional MD trajectory for trajectory-based analysis.
- `diffE.dat`: Target displacement vector calculated between the initial and final structures.
- `initial_hessian.dat`: Hessian matrix calculated from the initial structure for structure-based analysis.
- `initial_inv_hessian.dat`: Inverse Hessian matrix used as the response matrix in structure-based analysis.
- `results.txt`: Output file containing ranked MPR solutions.
- `bild_files/`: Directory containing ChimeraX-compatible `.bild` files generated for force-vector visualization.
- `perturbation_vector_k1_rank1.bild`: ChimeraX-compatible visualization file for the top-ranked k = 1 solution.
- `perturbation_vector_k2_rank1.bild`: ChimeraX-compatible visualization file for the top-ranked k = 2 solution.
- `perturbation_vector_k3_rank1.bild`: ChimeraX-compatible visualization file for the top-ranked k = 3 solution.

## Software requirements

Recommended software and packages:

- Python 3.8
- NumPy
- SciPy
- Matplotlib
- Biopython
- ProDy
- Jupyter Notebook
- ChimeraX
- Gurobi Optimizer, only for optimization-based MPR calculations

The provided `environment.yml` file installs the Python 3.8 environment required for the ProDy-based structure calculations. Gurobi is not included in `environment.yml` because Gurobi package availability for Python 3.8 depends on platform and installation method. Enumeration-based analyses for small `k` values can be performed without Gurobi. Optimization-based MPR analyses require Gurobi Optimizer, `gurobipy`, and a valid Gurobi license.

## Installation

Clone the repository:

```bash
git clone https://github.com/midstlab/MPR_Bio-Protocol.git
cd MPR_Bio-Protocol
```

Create and activate the recommended conda environment using the provided `environment.yml` file:

```bash
conda env create -f environment.yml
conda activate mpr
```

The `environment.yml` file installs the Python 3.8 environment required for the ProDy-based structure calculations.

Alternatively, the required Python packages can be installed manually:

```bash
conda create -n mpr python=3.8
conda activate mpr
pip install numpy scipy matplotlib biopython prody jupyter
```

Install Gurobi separately if optimization-based MPR analysis is required. Gurobi is not included in `environment.yml` because its package availability for Python 3.8 depends on platform and installation method. To use optimization-based MPR, ensure that `gurobipy` is available in the active Python environment and that a valid Gurobi license is installed.

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open:

```text
MPR.ipynb
```

## Input files

### Required input files

- `initial.pdb`: Initial-state protein structure.
- `final.pdb`: Final-state protein structure.

The initial and final structures must contain the same number of Cα atoms in the same order. If the structures contain missing residues, unresolved loops, different chain lengths, or mismatched termini, retain only the common residue range before running the analysis.

### Optional input file

- `initial.dcd`: Equilibrated MD trajectory of the initial state.

For the provided FBP example, `initial.dcd` corresponds to a 40-ns equilibrated plateau segment extracted from a 100-ns MD simulation of the initial apo state. This trajectory is used for covariance-matrix calculation in trajectory-based MPR analysis.

## Configuration

Edit the following variables directly in the notebook.

### Input files

```python
FINAL_PDB = "final.pdb"
INITIAL_PDB = "initial.pdb"
INITIAL_DCD = "initial.dcd"  # optional; 40-ns plateau trajectory for trajectory-based analysis
```

If no DCD file is provided, the workflow uses the structure-based mode.

### Perturbation range

```python
Kmin = 1
Kmax = 3
```

`Kmin` and `Kmax` define the minimum and maximum number of residues perturbed simultaneously. Start with `k = 1` to obtain the single-residue solution, and then increase `k` to evaluate whether multiple residues improve the overlap. In practice, `k` should be increased until overlap values reach a plateau or the improvement becomes marginal.

### Optimization settings

Optimization-based MPR uses a Gurobi model defined in the notebook. Runtime-related parameters, such as time limit and optimality gap, can be adjusted at the point where model parameters are set to shorten computation time for larger systems or higher `k` values.

The scaling parameter `M` is estimated from enumeration-based calculations for small `k` values. These runs provide a reasonable range for force magnitudes and help reduce numerical instability during optimization.

## Running the workflow

1. Place `initial.pdb`, `final.pdb`, and optionally `initial.dcd` in the working directory.
2. Open `MPR.ipynb`.
3. Edit the input file names and perturbation range.
4. Run the structure preparation and superimposition cells.
5. Run the displacement-vector calculation cells.
6. Run the structure-based branch if only PDB files are used, or the trajectory-based branch if `initial.dcd` is provided.
7. Run the MPR overlap calculation cells.
8. Generate `.bild` visualization files.
9. Open the `.bild` files in ChimeraX together with the protein structure.

## Output

### `results.txt`

`results.txt` contains ranked MPR solutions, including:

- Overlap values (Omax)
- Residue indices (1-based)
- Perturbation vectors
- Results for each tested `k` value

Residue indices are 1-based and correspond to the processed Cα atom list used in the analysis. If the PDB residue numbering does not start at 1, map the output indices back to the original PDB numbering.

### `.bild` files

ChimeraX-compatible `.bild` files are generated from selected MPR solutions for force-vector visualization. In the example dataset, the repository includes the top-ranked visualization files for k = 1, k = 2, and k = 3:

- `perturbation_vector_k1_rank1.bild`
- `perturbation_vector_k2_rank1.bild`
- `perturbation_vector_k3_rank1.bild`

The generated `.bild` files are saved in the `bild_files/` directory.

## Interpreting results

Higher overlap values indicate that the predicted response better reproduces the target conformational displacement. Interpretation should be based on trends across `k` values. A high overlap value for `k = 1` suggests that a single dominant perturbation can reproduce the transition, whereas improved overlap at higher `k` values suggests cooperative multi-residue control.

If overlap values remain low for all tested `k` values, check structural alignment, residue matching, trajectory equilibration, and whether the selected initial and final structures represent a physically meaningful transition.

## Visualization in ChimeraX

Open the initial structure and the generated `.bild` file in ChimeraX:

```text
open initial.pdb
open perturbation_vector_k1_rank1.bild
```

The `.bild` file displays the selected residues and optimized force vectors. The same procedure can be used for the k = 2 and k = 3 visualization files.

## Notes

- The initial and final structures must contain the same number and order of Cα atoms.
- For trajectory-based analysis, use an equilibrated trajectory segment.
- Optimization-based MPR requires Gurobi, `gurobipy`, and a valid Gurobi license. Enumeration-based calculations for small `k` values can be performed without Gurobi.

## Citation

If you use this protocol or code, please cite the associated Bio-protocol article and related MPR publication.