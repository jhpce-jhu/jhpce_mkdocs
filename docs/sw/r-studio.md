---
tags:
  - in-progress
---  
# R Studio

We have a version of R available as a [module](modules.md).

This version of R is also known as conda_R. It was built with conda and contains many extra packages commonly used on the cluster.

You can install your own packages to your home directory, as described in [this document about building your own R and python pkgs](adding-pkgs.md). You can also install your own version of R in your home directory, preferably using miniconda.

## Running RStudio
Unless you request memory, a job is given 5G by default. That may be inadquate for R Studio.

- `srun --pty --x11 --mem=10G bash`
- `module load conda_R`
- `module load rstudio`
- `rstudio &`
