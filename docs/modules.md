# Environment modules
## Introduction

The JHPCE cluster uses modulefiles to allow users to configure their
shell environments. Some applications will not run until you load the
corresponding modulefile. A handful of widely used modulefiles are
loaded by default when you log into the cluster (e.g., SGE, R, gcc,
perl etc.). To see what modules are loaded you can enter the following
command at the shell prompt:

```
module list
```

Modulefiles cure the the age-old headaches associated with configuring
paths, environment variables and different software versions. For
example, gcc or open64 compilers need different libraries. Some users
need python 2.6 while other users need python 2.7 or python 3. Some
users want a standard stable R, while some want the latest and
greatest development version of R that was compiled the night before.

We use the lmod modulefile system developed at the Texas Advanced
Computing Center (TACC).

## Basic module commands for users

A modulefile is a script that sets up the paths and environment
variables that are needed for a particular application or development
environment. Most users will just use our modulefiles. But no doubt
some of you will want to finely control your shell environment. In
which case you can start developing your own custom modulefiles. There
are six basic commands that users should know

```
module list                 # list your currently loaded modules
module load   <MODULEFILE>  # configures your environment according to modulefile 
module unload <MODULEFILE>  # rolls back the configuration performed by the associated load
module avail                # shows what modules are available for loading
module swap <OLD> <NEW>     # unloads <OLD> modulefile and loads <NEW> modulefile
module initadd <MODULEFILE> # configure a module to be loaded at every login
module spider               # lists all modules, including ones not in your MODULEPATH
module spider <NAME>        # search for a module whose name includes <NAME>
Please refer to the TACC documentation for more details.
```

## Defaults

By default the following modules are loaded on all compute hosts and the login hosts when you log in

```
sge/2011.11p1  # allows access to grid engine commands
gcc/4.4.7      # environment for gcc 4.4.7
R/all          # environment for all versions of R. Visit our R page
perl/5.10.1    # perl 5.10.1 and libraries. Visit our perl page
By default the following modules are loaded on all compute hosts

matlab         # environment for matlab
stata          # environment for stata
By default the following modules are loaded only on the hosts of the appropriate queue

sas            # loaded on the sas.q host
mathematica    # loaded on the math.q
Configuring your .bashrc
```

It is critical that your `.bashrc` file sources the system-wide bashrc
file. Otherwise nothing will work! After you source the system-wide
bashrc file you can tailor your environment variables for any
applications or versions that you want. For example:

```
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
no applications as a service center. Instead we encourage and
facilitate power users to maintain their tools in a manner that makes
their tools available to all users. These users maintain their
applications as well as the corresponding modulefiles. Below is a list
of applications and application suites that are maintained by
community maintainers. Please refer to their documentation for
details. Also please be considerate. Maintaining software for you is
not their day job. There is absolutely no point in getting bent out of
shape if they can’t (or won’t) service your request.

```
Description	Maintainer	Documentation
R	Kasper Hansen	Documentation
Perl	Jiong Yang	Documentation
Python	Alyssa Frazee	Documentation
ShortRead Tools	Kasper Hansen	Documentation
```

## Module frequently asked questions

### I’m on a Mac, and the `~C` command to interrupt an ssh session isn’t working
  
  It used to, but I upgraded MacOS and now it does not work.*
  Some versions of MacOS have disable by default the ability to send
  an SSH Escape with `~C`.  To reenable this, on you Mac, you need to
  set the `EnableEscapeCommandline` option.  You can do this by either
  running `ssh -o EnableEscapeCommandline=yes . . .` or by editing
  your `~/.ssh/config` file, and at the top of that file add the line:

```
EnableEscapeCommandline=yes
```

### How do I get the Rstudio program to work on the cluster?

Use the app-portal [https://jhpce-app02.jhsph.edu/](https://jhpce-app02.jhsph.edu/).


### My X11 forwarding stops working after 20 minutes 

This error comes from the `ForwardX11Timeout` variable, which is set
by default to 20 minutes.  To avoid this issue, a larger timeout can
be supplied on the command line to, say, 336 hours (2 weeks):

```
$ ssh -X username@jhpce01.jhsph.edu -o ForwardX11Timeout=336h
```
### How do I copy a large directory structure from one place to another.

As an example, to copy a directory tree from `/home/bst/bob/src` to
`/dcs01/bob/dst`, first, create a cluster script, let’s call it
`copy-job`, that contains the line `rsync -avzh /home/bst/bob/src/
/dcs01/bob/dst/`. Next, submit this script as a batch job to the
cluster.

### My app is complaining that it can’t find a shared library, e.g. `libgfortran.so.1` 

Nine times out of ten, the allegedly missing library is there. The
problem is that your application is looking for the version of the
library that is compatible with the old system software. It will not
help to point your application to the new libraries. They are more
than likely to be incompatible with the new system. The correct
solution is to reinstall your software. If the problem persists after
the reinstallation, then please contact us and we will install
standard libraries that are actually missing.

### ssh gave a scary warning: `REMOTE HOST IDENTIFICATION HAS CHANGED`

Go into the `~/.ssh` directory of your laptop/desktop and edit the
known_hosts file.  Search for the line that starts with the host that
you ssh’d to. Delete that line (it is probably a long line that
wraps). Then try again

### Why aren’t SLURM commands, or R, or matlab, or… available to my cron job?

`cron` jobs are not launched from a login shell, but the module
commands and the JHPCE default environment is initialized
automatically only when you log in. Consequently, in a cron job, you
have to do the initialization yourself. Do this by wrapping your cron
job in a bash script that initializes the module command and then
loads the default modules. You bash shell script should start with the
following lines:

```
#!/bin/bash

# Source the global bashrc
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi
module load JHPCE_DEFAULT_ENV
```
