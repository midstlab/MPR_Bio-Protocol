{\rtf1\ansi\ansicpg1252\cocoartf2867
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;\f1\fnil\fcharset0 LucidaGrande;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\paperw11900\paperh16840\margl1440\margr1440\vieww39140\viewh8400\viewkind0
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural\partightenfactor0

\f0\fs24 \cf0 # Multiply Perturbed Response to Disclose Allosteric Control of Conformational Change**\
\
This repository contains the Jupyter notebook and supporting code used to compute Multiply Perturbed Response (MPR) solutions for a known protein conformational transition (e.g. apo 
\f1 \uc0\u8594 
\f0  holo).\
\
\
## Description\
\
The MPR method identifies single or multiple residues whose collective perturbation best reproduces an experimentally observed conformational change between two protein states.\
\
The workflow supports:\
- Structure-based analysis using two PDB files\
- Trajectory-based analysis using an MD trajectory (DCD)\
- Enumeration and optimization-based solutions for multi-residue perturbations\
\
Jupyter notebook implementing the full MPR workflow, including:\
  - Structure superimposition\
  - Displacement vector calculation\
  - Covariance / inverse Hessian construction\
  - MPR overlap (Omax) calculations\
  - Output parsing and visualization file generation\
\
\
## Configuration\
\
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
  To use optimization, a Gurobi license should be generated and downloaded.\
\
## Output\
\
results.txt\
  Ranked MPR solutions including:\
  - Overlap values (Omax)\
  - Residue indices (1-based)\
  - Perturbation vectors\
\
bild_files/\
  ChimeraX-compatible .bild files generated from selected MPR solutions for force-vector visualization.\
\
\
\
## REFERENCE\
\
Berksoz, M., et al. (2025). Multiply Perturbed Response to Disclose Allosteric Control of Conformational Change: Application to Fluorescent Biosensor Design. Journal of Molecular Biology.\
\
}