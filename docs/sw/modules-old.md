---
tags:
  - needs-review
  - jiong
---

# Environment modules

!!! Note "Authoring Note"
    The [last section](modules.md/#community-maintained-applications) needs to be updated for 2024. LIBD is not mentioned, documentation is not linked correctly anymore, etc. Convert to a Markdown table?

## Introduction

The JHPCE cluster uses the [Lmod](https://lmod.readthedocs.io/en/latest/) module system to allow users to configure their
shell environments. {==Some applications will not run until you load the
corresponding modulefile.==} 

A handful of widely used modulefiles are
loaded by default when you log into the cluster. To see what modules are loaded you can enter the following
command at the shell prompt:

``` linenums="0"
module list
```
The naming convention for modules is "software_name/version" (e.g. bowtie/2.5.1). If a module is loaded, it will be followed by `(L)` The default version, if designatedk will be followed by a `(D)`

Modules cure the the age-old headaches associated with configuring
paths, environment variables and different software versions. For
example, gcc or open64 compilers need different libraries. Some users
need python 3.9 while other users need python 3.10. Some
users want a standard stable R, while some want the latest and
greatest development version of R that was compiled the night before.

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

## Community maintained applications

We use a community-based model of application maintenance to support
our diverse user base. Briefly, this means that we support essentially
few applications as a service center. Instead we encourage and
facilitate power users to maintain their tools in a manner that makes
their tools available to all users. These users maintain their
applications as well as the corresponding modulefiles. Below is a list
of applications and application suites that are maintained by
community maintainers. Please refer to their documentation for
details. Also please be considerate. Maintaining software for you is
not their day job. There is absolutely no point in getting bent out of
shape if they can’t (or won’t) service your request.

``` linenums="0"
Description	Maintainer	Documentation
R	Kasper Hansen	Documentation
Perl	Jiong Yang	Documentation
Python	Alyssa Frazee	Documentation
ShortRead Tools	Kasper Hansen	Documentation
```
