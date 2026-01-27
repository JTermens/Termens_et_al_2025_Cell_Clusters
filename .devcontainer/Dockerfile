# Dockerfile
#
# Simple Dockerfile to execute a demo simulation for the paper
# Termens et al. "Cell clusters sense their global shape to drive collective
# migration" (2025)
#
# Author: Joan Térmens (https://github.com/JTermens)
# Last update: 26th of january 2026
#
# We pull the official FreeFem++ image available, which runs an Ubuntu 22.04.1 LTS
# environment with FreeFem++ v4.12 (2022 Dec 7th) installed. 
#
# The container is meant to execute the demo, not for interactive development. For a
# complete development environement, refer to the latest FreeFem++ version at 
# https://github.com/FreeFem/FreeFem-sources/releases.
#
# If you also need an editor inside the container, install one manually 
# (e.g. apt-get install neovim).

# Pin the version to avoid compatibility problems.
FROM freefem/freefem:v4.12

# Define the work directory inside the container
WORKDIR /workdir


