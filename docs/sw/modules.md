---
markdown_extensions:
  - pymdownx.highlight
  - pymdownx.inlinehilite
---

## **Environment Modules**

### Introduction

Environment Modules provide a convenient way to dynamically change the users’ environment through modulefiles. When a user loads a module for a specific version of a software package, appropriate changes are made to the user's environment, such as adding new locations in the PATH environment variables. When a package is no longer needed, users should unload the module from their environment, (to prevent interactions between module settings).

The JHPCE cluster uses the [Lmod](https://lmod.readthedocs.io/en/latest/) module system to allow users to configure their shell environments.

### Use modules on JHPCE cluster

The naming convention for modules is "software_name/version" (e.g. bowtie/2.5.1). If a module is loaded, it will be followed by `(L)` The default version, if designatedk will be followed by a `(D)`

- list loaded modules in your environment
<pre><code>[test@compute-107 ~]$ <span style="background-color:yellow">module list</span>

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0
</code></pre>

``` shell-session
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

``` hl_lines="6"
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

## Basic module commands for users

A module file is a script that sets up the paths and environment
variables that are needed for a particular application or development
environment. Most users will just use our modulefiles. But if you want to finely control your shell environment, you can start developing your own custom module files. 

There
are a few basic commands that users should know:

``` linenums="0"
module list                 # list your currently loaded modules
module load   <MODULEFILE>  # configures your environment according to modulefile 
module unload <MODULEFILE>  # rolls back the configuration performed by the associated load
module avail                # shows what modules are available for loading
module help                 # 
module spider               # lists all modules, including ones not in your MODULEPATH
module spider <NAME>        # search for a module whose name includes <NAME>
module save <NAME>.         # save currently-loaded modules to "default" or optionally to <NAME>
module restore <NAME>.      # load the saved collection
```

## Defaults

By default the following modules are loaded on all compute hosts and the login hosts when you log in

``` linenums="0"
JHPCE_ROCKY9_DEFAULT_ENV
JHPCE_tools/3.0
```

## Configuring your .bashrc

It is critical that your `.bashrc` file sources the system-wide bashrc
file. Otherwise basic programs will not work! After you source the system-wide
bashrc file you can add code which will modify your environment. For example:

```bash linenums="0"
# Always source the global bashrc
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi

# If I prefer gcc/4.8.1 as my default compiler
module load gcc/4.8.1
```
