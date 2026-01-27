# Termens_et_al_2025_Cell_Clusters

This repository contains a minimal and self‑contained demo of the numerical simulations 
associated with the manuscript:

> Termens et al. (2025)
> "Cell clusters sense their global shape to drive collective migration"

The purpose of this repository is transparency and reproducibility: it allows referees 
and readers to inspect the numerical pipeline, execute a representative simulation, and
reproduce the figures shown in the manuscript.

It does not pretend to be a general‑purpose simulation framework, nor a production‑ready 
research codebase. The focus is transparency and faithfulness to the published results.

## Repository structure

```
.
├── .devcontainer/                      # Containerized FreeFem++ environment
├── solver_pv.edp                       # Main FreeFem++ script
├── lib/                                # Folder with utility script for solver_pv.edp
│   ├── mesh-generation.edp             # Mesh generation utilities
│   └── remeshing.edp                  # Boundary remeshing utilities
├── semicircle_test.tar.xz/             # Example output from a reference simulation
│   ├── params.csv                      # Plot of the first and last traction frames
│   ├── global_sol.csv                  # Plot of the first and last velocity frames
│   ├── msh/                            # Folder with the output meshes in .msh format
│   │   ├── mesh_1000000.msh
│   │   ├── ...
│   │   └── mesh_1001980.msh
│   └── local_sol/                      # Folder with the output solutions in .txt format
│       ├── sol_1000000.txt
│       ├── ...
│       └── sol_1001980.txt
├── post-processig.nb                   # Wolfram Mathematica post-processing notebook
├── semicircle_test_plots/              # Plots generated from the reference simulation
│   ├── global_variable.png             # Fig showing the evolution of some integrals
│   ├── taction_frames.png              # Plot of the first and last traction frames
│   ├── velocity_frames.png             # Plot of the first and last velocity frames
│   └── gifs/                           # Boundary remeshing utilities
│       ├── semicircle_test_y-trac.gif  # Evolution of the local cluster traction
│       └── semicircle_test_y-vel.gif   # Evolution of the local cluster traction
└── README.txt               # This file
```

---

## Overview of the numerical pipeline

The demo reproduces a representative simulation used in the manuscript. The numerical 
pipeline is devided between the following scripts:

1. Geometry and mesh generation
   Implemented in `mesh-generation.edp`, defining the initial cluster shape and boundary
   discretization.

2. Adaptive remeshing
   Implemented in `remeshing.edp`, ensuring numerical stability and resolution during 
   boundary deformation.

3. Physical model and solver
   Implemented in `solver_pv.edp`, which defines physical and numerical parameters,
   solves the governing equations using FreeFem++ and writes results to disk.

4. Post‑processing and visualization
   Implemented in `post-process.nb` (Wolfram Mathematica), producing some similar plots 
   to those in the manuscript.


## Requirements

To run the demo natively, you need a Unix-like environment (Linux, macOS) with FreeFem++ 
v4.12 (or compatible) installed. Running the post‑processing notebook requires Wolfram 
Mathematica in a reasonably recent version.

To avoid local installation issues, a containerized environment is provided (see below).

## Running the simulation

### Using the provided devcontainer

The `.devcontainer/` directory defines a minimal container environment based on the 
official FreeFem++ docker image. It runs Ubuntu 22.04 LTS with FreeFem++ v4.12 installed 
and mounts this repository into `/workdir`. We recommed to use this method in order to 
ensure replicability and ease of use.

The easyest method to run repository within the devcontainer environment is to use the 
`vscode` text editor. Upon opening the repository in `vscode` a notification asking to 
install the devcontainer extension will appear in the bottow-right corner. Then, a new 
notification will pop offering to open the repository in the devcontainer. After building 
the environment you will be able to run the simulations. Even though using `vscode` is 
recommended for its ease of use, the devcontainer is intentionally editor‑agnostic and 
can be used with multiple tools, like Docker, Podman or DevPod. As an example, 

```bash
cd /Termens_et_al_2025_Cell_Clusters
devpod up .
devpod ssh
cd /workdir
```
runs the simulation in the devcontainer using the later. All three methods are 
functionally equivalent.

## Executing the simulation on FreeFem++

Either inside the provided container or locally, if FreeFem++ is already installed, you 
can simply run the simulations with:

```bash
FreeFem++ solver_pv.edp -v 0
```
where the verbosity is set to zero by default to reduce output noise in a demo context. 
You could also run the simulation inside the devcontainer in `vscode` by either typing 
<Super>+<Shift>+<r> or opening a terminal.

The logic of the numerical method and the governing equations are detailed in 
B. Numerical Scheme within the METHODS section of the cited paper. To further look for 
implementation details, consult the script comments at `solver_pv.edp`, 
`lib/mesh-generation.edp` and `lib/remeshing.edp`.

## Output and reference results

Running the solver creates the output directory `semicircle_test/` as specified internally 
in `solver_pv.edp`. To inspect the expected output format and compare your own results 
against a known reference, the repository already contains the compressed folder 
`semicircle_test.tar.xz` with results from a reference run.

## Post‑processing with Wolfram Mathematica

The file `post-processing.nb` is a Wolfram Mathematica notebook used to load the data at 
`semicircle_test.tar.xz` and generate the figures associated with the demo simulation. 
The plots generated by the notebook should match those in `semicircle_test_plots/`. Take 
into account that the notebook is intentionally explicit rather than optimized, to favor 
readability and transparency.

## Notes on scope and limitations

* This demo illustrates one representative parameter set.
* The post-processing code prioritizes clarity over performance.
* The numerical parameters are chosen for robustness and reproducibility, not exhaustive 
exploration.

The repository is meant to see exactly how the simulations behind this paper were run, 
rather than to provide a complete research and development framework.

---

## Contact

Joan Térmens
GitHub: [https://github.com/JTermens](https://github.com/JTermens)
