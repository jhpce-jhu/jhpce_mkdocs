---
tags:
  - needs-to-be-written
  - jeffrey
  - slurm
---

# SLURM Job Environments

!!! Warning
    There are useful things we can document about controlling the job execution environment. This is fairly low priority though, as most things Just Work. 
    
    
## Overview of what might be covered here

Some parts of the environment in the shell you submit a job are copied into the job's environment. This can be controlled with certain SLURM arguments.

Some clusters strongly advise their users to create batch scripts in which the `#!/bin/bash` first line has a `-l` flag. Because shells like bash process "dot files" (e.g. `.profile` and .`bashrc`) differently for login versus interactive shells. Perhaps this can explain subtle behaviors you notice. Perhaps this is more important in some clusters than others because of the way accounts are provisioned when created.

SLURM sets a large number of shell environment variables for jobs to consult if desired.  A good list can be found in the sbatch manual page's INPUT ENVIRONMENT VARIABLES and OUTPUT ENVIRONMENT VARIABLES sections.

You can see some of them by starting an interactive session and running `printenv | grep -i slurm` (there may be others that are set for jobs depending on their type and arguments -- array jobs, dependent jobs, ???).

The way SLURM commands operate can be influenced by setting some certain environment variables, such as SLURM_TIME_FORMAT, SACCT_FORMAT, SQUEUE_FORMAT, SQUEUE_FORMAT2, SQUEUE_SORT. It can be useful to define these in aliases or shell scripts to format output in ways you need. Simply changing the value of these variables can produce vastly different output for commands like sacct and squeue.

Are there any tricks to propagating information between components of a job? I mean setting your own variables in batch job scripts and having them be visible by all of the processes you mean to use them?

When modifying environment variables in shell scripts or your current shell, remember that you may be dealing with existing information that you don't want to discard (by redefining the variable without referring to the existing value). When dealing with variables that you don't make up out of whole cloth and which have simple names which might match existing ones, check for their existence first. If they exist, you may want to prepend your changes, say to a PATH-type variable which usually has multiple entries. Or append, if you want system defaults to come *before* your values.

Remember that [modules](../sw/modules.md) change environment variables when loaded and unloaded. User's default environments are controlled by some JHPCE-created modules. 

## Until this is fleshed-out...

Consult [our list](../slurm/user-guide-collection.md) of good documentation at other clusters.

Send suggestions of items to include here to bitsupport.
