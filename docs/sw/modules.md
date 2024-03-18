## **Environment Modules**

### Introduction

Environment Modules provide a convenient way to dynamically change the users’ environment through modulefiles. When a user loads a module for a specific version of a software package, appropriate changes are made to the user's environment, such as adding new locations in the PATH environment variables. When package is no longer needed, users can unload the module from their environment.  

The JHPCE cluster uses the [Lmod](https://lmod.readthedocs.io/en/latest/) module system to allow users to configure their shell environments.

### Use modules on JHPCE cluster
- list loaded modules in your environment
```
[test@compute-107 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0
```

- list available modules
```
[test@compute-107 ~]$ module avail
```

!!! Warning "Notes:"  
    * in order to use a software that is not in shell's standard search path, you need first load its module  
    * LIBD contributes many modules. If you need help with any of these, please email bithelp

- load a module (e.g. R module)
```
[test@compute-107 ~]$ module load R
Loading R/4.3
(4.3)[test@compute-107 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) conda/3-23.3.1   4) R/4.3
```

  Note: Some software packages depend on other software. Loading module for the package may load modules for the other software as well. In this case, loading R also loads conda module. If you have your own conda environment, please make sure there is no conflicts.

- unload a module (e.g. when R is no longer needed, you can unload it from your environment)
```
(4.3)[test@compute-107 ~]$ module unload R
Unloading R/4.3
[test@compute-107 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) conda/3-23.3.1
```

- get help on using module
```
[test@compute-107 ~]$ module help
Usage: module [options] sub-command [args ...]
...
```
