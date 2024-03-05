---
tags:
  - needs-to-be-written
---
# Adding Your Own Python and R Libraries

## How to add your own packages to R and python
I think our user community would welcome instructions on how to add pkgs to R and python, at the minimum.

There are right and wrong ways to do this.

There are implications of loading modules in which order before doing things.

Do you create a conda environment first, or a python virtual environment?

What information do other HPC sites deem important to tell their users? Are there examples among the pages for listed TOWARDS THE BOTTOM in [this github issue](https://github.com/jhpce-jhu/jhpce/issues/764)?

A GOOD EXAMPLE
Example from [C.E.C.I](https://support.ceci-hpc.be/doc/_contents/UsingSoftwareAndLibraries/InstallingSoftwareByYourself/index.html)

## R
FIRST YOU MUST LOAD R MODULE
`module load R`

If you try to load a library and it is not found, then you can install one in your home directory.

```R linenums="0"
> install.packages("sf")
> install.packages('terra', repos='https://rspatial.r-universe.dev')
> library('sf')
> library('terra')
```

You should recompile your libraries when the version of R changes.

How do you recompile instead of install?

When installing R packages from source with compiled programs, you can add custom compiler flags in ~/.R/Makevars. Adding optimization flags may provide a boost in performance for some packages. 
```
STDFLAGS = -O2 -pipe -Wall 
```

We have a wide variety of CPU architecture across the cluster, so you probably don't want to add to STDFLAGS `-march=` and `-mtune=` arguments.



## Python
