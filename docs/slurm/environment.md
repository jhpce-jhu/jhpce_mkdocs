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

### Set by SLURM
SLURM sets a large number of shell environment variables for jobs to consult if desired.  A good list can be found in the sbatch manual page's INPUT ENVIRONMENT VARIABLES and OUTPUT ENVIRONMENT VARIABLES sections.

You can see some of them by starting an interactive session and running `printenv | grep -i slurm` (there may be others that are set for jobs depending on their type and arguments -- array jobs, dependent jobs, ???).

### Managing output of sacct and squeue
The way SLURM commands operate can be influenced by setting some certain environment variables, such as SLURM_TIME_FORMAT, SACCT_FORMAT, SQUEUE_FORMAT, SQUEUE_FORMAT2, SQUEUE_SORT. It can be useful to define these in aliases or shell scripts to format output in ways you need. Simply changing the value of these variables can produce vastly different output for commands like sacct and squeue.

### Existing environment
Are any parts of the environment in the shell you submit a job from copied into the job's environment? This can be controlled with the `--export` argument.

Some clusters strongly advise their users to create batch scripts in which the `#!/bin/bash` first line has a `-l` flag. Because shells like bash process "dot files" (e.g. `.profile` and .`bashrc`) differently for login versus interactive shells. Perhaps this can explain subtle behaviors you notice. Perhaps this is more important in some clusters than others because of the way accounts are provisioned when created. You can test the difference in behavior by submitting trivial test jobs that run a command to save the environment of the job (`printenv > my-env.txt`) and compare them.

### Passing info into jobs

It is possible to pass variables into a SLURM job when you submit the job using the `-–export` flag. For example to pass the value of the variables A and b into the job script script.sh you can use:

`sbatch --export=A=25,b='test' script.sh`

```Shell title="Inside of script.sh"
#!/bin/bash
echo ${A}
echo ${b}
```

or use the special keyword `ALL` (although one can imagine that possibly going awry)

`sbatch --export=ALL,A=7,b='test3' script.sh`

### Using variables to set SLURM job name and output files

**SLURM does not support using variables in the #SBATCH lines within a job script.** However, values passed from the command line have precedence over values defined in the job script. So the job name and output/error files can be passed on the sbatch command line:

```
A=70
b='trial-24'

sbatch --job-name=${A}.${b}.run --output=${A}.${b}.out --export=D=${A},f=${b} script.sh
```

Are there any tricks to propagating information between components of a job? I mean setting your own variables in batch job scripts and having them be visible by all of the processes you mean to use them? This [UPenn page](https://hpc.seas.upenn.edu/home/mp-clusters/how-to-use-mp-clusters-slurm/advanced-slurm-options/) in item number 2 suggests creating a shell script that you run within an sbatch file:

"In certain situations (e.g. multiple nodes), the scriptfile itself will not see your environment variable. To get around this, separate your submission into a control file that you submit to sbatch, and a script file that is actually run within each task slot of your job. For example, if you want to run sbatch –export=MYVARIABLE controlfile, OR you have an environment variable MYVARIABLE already set and you just run sbatch controlfile, then your controlfile would have your regular #SBATCH headers and one command: srun scriptfile. This makes sure that your entire environment is transferred to the scriptfile on every job step, inside every task.""



!!! Warning
    When modifying environment variables in shell scripts or your current shell, remember that you may be dealing with existing information that you don't want to discard (by redefining the variable without referring to the existing value). When dealing with variables that you don't make up out of whole cloth and which have simple names which might match existing ones, check for their existence first. If they exist, you may want to prepend your changes, say to a PATH-type variable which usually has multiple entries. Or append, if you want system defaults to come *before* your values.

Remember that [modules](../sw/modules.md) change environment variables when loaded and unloaded. User's default environments are controlled by some JHPCE-created modules. 

## Until this is fleshed-out...

Consult [our list](../slurm/user-guide-collection.md) of good documentation at other clusters.

Send suggestions of items to include here to bitsupport.
