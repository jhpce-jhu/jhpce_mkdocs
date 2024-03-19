## Installing R packages

- load R module from a compute node
```
[test@compute-107 ~]$ module load R
Loading R/4.3
(4.3)[test@compute-107 ~]$
```
!!! Note "Note"
    R points to conda_R. Therefore, loading either R or conda_R would use the same version of R.
    
- use `install.package(pkgname)` to install a package
```
(4.3)[test@compute-107 ~]$ R

R version 4.3.1 Patched (2023-07-19 r84711) -- "Beagle Scouts"
Copyright (C) 2023 The R Foundation for Statistical Computing
Platform: x86_64-conda-linux-gnu (64-bit)
...
[Previously saved workspace restored]

> install.package("your-package-name")
```
