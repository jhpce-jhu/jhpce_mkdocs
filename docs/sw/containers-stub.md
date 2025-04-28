---
tags:
   - needs-to-be-written
   - mark
---
# Containers

## Using Our Container Solutions
We have two solutions that you can use (R Studio Server, JupyterHub). See which documents about using them???

Maybe this is a good time to at least get credit for the work Mark has done building the singularity containers we now offer.

What do you need to know about using them? Quitting them? Waiting how long for them to finish starting?

What verbiage about this can be shared with Web Portal docs?

## Building Your Own Containers

Maybe we can document this later in terms of how users can build them. In the mean time, provide some links??

They don't have root access, so what is that trick that allows you to make a container image?

### How do you run a batch SLURM job using a container?

Where is the best file system to host a container file if you're going to run it in batch mode on multiple nodes? /fastscratch? Does it make a difference?


## Examples from Elsewhere

The [Frontera](https://docs.tacc.utexas.edu/hpc/frontera/#containers) site's section on containers mentions something that users building their own containers on JHPCE need to know to add to their containers -- e.g. which file systems to include.

[NERSC](https://docs.nersc.gov/development/containers/podman-hpc/podman-beginner-tutorial/) has a well-written set of pages about using [podman-hpc](https://docs.nersc.gov/development/containers/podman-hpc/overview/), which might serve as an example of just building and using containers even without podman-hpc

Zurich pages on [Singularity](https://doc.zih.tu-dresden.de/software/containers/) and [Recipes and Hints](https://doc.zih.tu-dresden.de/software/singularity_recipe_hints/)

USC pages on [Singularity](https://www.carc.usc.edu/user-information/user-guides/software-and-programming/singularity)
