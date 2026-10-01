# Termens_et_al_2025_Cell_Clusters

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23008219.svg)](https://doi.org/10.5281/zenodo.23008219)


This repository contains a minimal and self-contained demo of the numerical simulations associated with the manuscript:

> Térmens, J., Pi-Jaumà, I., Lavi, I., Matejčić, M., Fortunato, I. C., Trepat, X., and
> Casademunt, J. "Cell clusters sense their global shape to drive collective migration."
> arXiv preprint arXiv:2509.15910 (2025).

The purpose of this repository is transparency and reproducibility: it allows reviewers and readers to inspect the numerical pipeline, execute a representative simulation, and generate demo figures similar to those shown in the manuscript.

It does not pretend to be a general-purpose simulation framework, nor a production-ready research codebase. The focus is transparency and faithfulness to the published results.

## Repository structure

```
.
├── .devcontainer/                      # Containerized FreeFem++ environment
├── solver_pv.edp                       # Main FreeFem++ script
├── lib/                                # Folder with utility scripts for solver_pv.edp
│ ├── mesh-generation.edp               # Mesh generation utilities
│ └── remeshing.edp                     # Boundary remeshing utilities
├── semicircle_test.tar.xz/             # Example output from a reference simulation
│ ├── params.csv                        # Simulation parameters used in the reference run
│ ├── global_sol.csv                    # Time evolution of the global (integrated) observables
│ ├── msh/                              # Folder with the output meshes in .msh format
│ │ ├── mesh_1000000.msh
│ │ ├── ...
│ │ └── mesh_1001980.msh
│ └── local_sol/                        # Folder with the output solutions in .txt format
│ ├── sol_1000000.txt
│ ├── ...
│ └── sol_1001980.txt
├── post-processing.nb                  # Wolfram Mathematica post-processing notebook
├── semicircle_test_plots/              # Plots generated from the reference simulation
│ ├── global_variable.png               # Evolution of the global integrated observables
│ ├── traction_frames.png               # First and last traction frames
│ ├── velocity_frames.png               # First and last velocity frames
│ └── gifs/                             # Animated evolution of local fields
│ ├── semicircle_test_y-trac.gif        # Evolution of the local cluster traction
│ └── semicircle_test_y-vel.gif         # Evolution of the local cluster velocity
├── Source-Data.xlsx                    # Source data for Fig. 7e,f, Supp. Fig. S2a-j, S3a,b
├── LICENSE                             # GPL-3.0
└── README.md                           # This file
```

---

## Overview of the numerical pipeline

The demo reproduces a representative simulation used in the manuscript. The numerical pipeline is divided between the following scripts:

1. **Geometry and mesh generation**
   Implemented in [`mesh-generation.edp`](./lib/mesh-generation.edp), defining the initial cluster shape and boundary discretization.

2. **Adaptive remeshing**
   Implemented in [`remeshing.edp`](./lib/remeshing.edp), ensuring numerical stability and resolution during boundary deformation.

3. **Physical model and solver**
   Implemented in [`solver_pv.edp`](solver_pv.edp), which defines physical and numerical parameters, solves the governing equations using FreeFem++, and writes results to disk.

4. **Post-processing and visualization**
   Implemented in [`post-processing.nb`](./post-processing.nb) (Wolfram Mathematica), producing plots analogous to those in the manuscript.

## Requirements

To run the demo natively, you need a Unix-like environment (Linux, macOS) with FreeFem++ v4.12 (or compatible) installed. Running the post-processing notebook requires Wolfram Mathematica; the notebook was tested on version `14.3`.

To avoid local installation issues, a containerized environment is provided for FreeFem++ (see below). Mathematica cannot be containerized in this repository due to licensing restrictions.

## Running the simulation

### Using the provided devcontainer

The `./.devcontainer/` directory defines a minimal container environment pinned to a fixed FreeFem++ image (Ubuntu 22.04 LTS, FreeFem++ v4.12, referenced by digest for full reproducibility) and mounts this repository into `/workdir`. We recommend using this method to ensure replicability and ease of use.

The easiest method to run the repository within the devcontainer environment is to use the `vscode` text editor. Upon opening the repository in `vscode`, a notification asking to install the Dev Containers extension will appear in the bottom-right corner. A second notification will then offer to open the repository in the devcontainer. After building the environment you will be able to run the simulations.

The devcontainer is also editor-agnostic and can be run directly with Docker or DevPod. As an example, using DevPod:

```bash
cd Termens_et_al_2025_Cell_Clusters
devpod up .
devpod ssh
cd /workdir
```

## Executing the simulation on FreeFem++

Either inside the provided container or locally (if FreeFem++ is already installed), you can run the simulation with:

```bash
FreeFem++ solver_pv.edp -v 0
```
Verbosity is set to zero by default to reduce output noise in a demo context. Inside the devcontainer in `vscode`, you can also run the simulation via the `<Shift>+<Super>+<r>` task shortcut provided by the `vscode-FreeFEM` support package, or by opening a terminal directly.

Measured on a `AMD Ryzen 5 5625U with Radeon Graphics`, native Linux with Docker (no virtualization overhead) and 12 cores, the reference simulation takes approximately 1h 26min and peaks at ~0.6 GB of RAM.

The logic of the numerical method and the governing equations are detailed in the Numerical Scheme section from the Supplementary Information of the cited paper. For further implementation details, consult the script comments in [`solver_pv.edp`](./solver_pv.edp), [`lib/mesh-generation.edp`](./lib/mesh-generation.edp), and [`lib/remeshing.edp`](./lib/remeshing.edp).

## Output and reference results

Running the solver creates the output directory `semicircle_test/`, as specified internally in `solver_pv.edp`. To inspect the expected output format and compare your own results against a known reference, the repository already contains the compressed folder [`semicircle_test.tar.xz`](./semicircle_test.tar.gz) with results from a reference run.

## Post‑processing with Wolfram Mathematica

The file [`post-processing.nb`](./post-processing.nb) is a Wolfram Mathematica notebook used to load the data from [`semicircle_test.tar.xz`](./semicircle_test.tar.gz) and generate the figures associated with the demo simulation. The plots generated by the notebook should match those in [`semicircle_test_plots/`](./semicircle_test_plots/). The notebook is intentionally explicit rather than optimized, to favor readability and transparency.

## Source data

The source data underlying Fig. 7e,f, Supplementary Fig. S2a–j, and Supplementary
Fig. S3a,b are provided in [`Source-Data.xlsx`](./Source-Data.xlsx).

Each figure panel corresponds to a separate sheet in the file, named to match the panel label used in the manuscript.

## Notes on scope and limitations

* This demo illustrates one representative parameter set.
* The post-processing code prioritizes clarity over performance.
* The numerical parameters are chosen for robustness and reproducibility, not for exhaustive exploration.

This repository is meant to show exactly how the simulations behind this paper were run, rather than to provide a complete research and development framework.

## License

This repository is released under the GNU General Public License v3.0 (GPL-3.0). See
[LICENSE](./LICENSE) for details.

## Citation

If you use this code or data, please cite the associated preprint (see [CITATION.cff](./CITATION.cff)):

> Térmens, J., Pi-Jaumà, I., Lavi, I., Matejčić, M., Fortunato, I. C., Trepat, X., and
> Casademunt, J. "Cell clusters sense their global shape to drive collective migration."
> arXiv:2509.15910 (2025). https://arxiv.org/abs/2509.15910

This citation will be updated with the full journal reference upon publication.

## Acknowledgements

I would like to acknowledge Ido Lavi, Ph.D., for his invaluable help in conceiving the simulation methods and preparing an initial version of the codes here.

---

## Contact

Joan Térmens
ORCID: [0009-0002-2356-2113](https://orcid.org/0009-0002-2356-2113)
