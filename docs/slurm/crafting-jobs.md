---
tags:
  - in-progress
  - slurm
---

# Crafting SLURM Jobs

!!! Note "Authoring Note"
    This document is accumulating information which might best be split into several more-focused documents. There are many aspects of creating jobs.

There are a variety of techniques one can use to initiate SLURM jobs to accomplish various tasks. Here we will initially accumulate pointers to documentation written by others for their clusters. Later we will write concrete examples of our own.

In the directories under `/jhpce/shared/jhpce/slurm/` you will find files used during orientation, some accumulated documents and batch examples.

## Sbatch, srun and salloc

There are three commands used to request resources from SLURM. You will find all three discussed in the [linked documentation](slurm-commands-ref.md).

Here at JHPCE we have been using `srun` primarily as way to start interactive sessions. However it can also be used after allocating resources with `salloc` and inside of `sbatch` scripts. See web site [below](crafting-jobs.md#running-multiple-jobs-from-one-script) for examples.

Be careful using `salloc` that you don't leave allocated resources unused. One way to use salloc is to request resources and launch a new bash shell. Those resources are not released until that bash shell is ended.

## Sbatch Rules
1. First characters in batch file need to be: `#!/bin/bash` (although you can use an interpreter other than bash)
2. `#SBATCH` directives need to appear as the first characters on their lines.
3. `#SBATCH` directives need to appear before **_any_** shell commands.
4. You can put comments after # symbols.
4. You may need to add a `wait` command at the bottom to ensure that processes spawned earlier complete before the script does.

## SLURM Directive Order of Precendence
For now see these [two PDF pages](images/slurm-precendence.pdf) from an orientation document.

## Job Environment
By default jobs start in the same directory that was the current working directory when `sbatch` or `srun` were used. The directive `--chdir=somepath` or `#SBATCH --chdir=somepath` can be used to set the working directory of the job. Whether you choose to use this directive depends on your workflow and typical awareness of where you "are". Would you rather change batch files or set command line arguments to ensure that jobs run in the desired directories or would you rather double-check your location? 

We have a document that describes [other aspects of job environments](../slurm/environment.md).
    
## Input/output Considerations

!!! AuthoringNote 
    Use the same terms as used in the storage overview. We want to be consistent. It helps users, and helps us make links between related articles.

- home directory
- local compute node /tmp
- fastscratch
- project storage
    
Some programs have specific variables you can set to indicate where files should be created.

SAS has "WORK" -- is this set to something in the module load process?

R has "TEMPDIR" which defaults to /tmp.

```
You could use your 1TB of fastscratch space for this. So your SLURM script could use commands like:

module load conda_R
export TEMPDIR=$MYSCRATCH
R CMD BATCH myprog.R

```

## Dependent jobs

You can configure jobs to run in order with some conditional control. See [this part](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html#OPT_dependency) of the sbatch manual page.

## Heterogeneous Job Support

Each component of such jobs has virtually all job options available including partition, account and QOS (Quality Of Service). See this [vendor document](https://slurm.schedmd.com/archive/slurm-22.05.9/heterogeneous_jobs.html).

## Example Batch Jobs

### Copying data within cluster

Here is a sample batch job. More information about this topic is accumulating [here](../files/copying-files.md).

??? note "Click to expand"
    Content. How hard will it be to do code blocks?
    ```Shell
    #!/bin/bash

    #SBATCH -p shared
    #SBATCH --mem=10G
    #SBATCH --job-name=cp-files
    #SBATCH --time=15-0
    #SBATCH --nodes=1
    #SBATCH --ntasks=1
    #SBATCH --cpus-per-task=1
    #SBATCH --output=cp-files-%j.out	# file to collect standard output
    #SBATCH --error=cp-files-%j.err	# file to collect standard output
    #SBATCH --mail-user=my-email-address@jhu.edu
    #SBATCH --mail-type=BEGIN,FAIL,END

    date
    cd /dcs04/sample/path/

    echo "I am running on compute node:"
    hostname

    echo "In directory:"
    pwd

    echo "The files found in this directory are:"
    /bin/ls

    # args are meant to try to prevent files from being deleted in destination
    rsyncargs="-h --progress --sparse --numeric-ids --one-file-system --stats --ignore-existing --max-delete=0"

    echo "about to try to rsync"

    rsync -a $rsyncargs directory-to-be-copied /dcs05/destination/path/

    date
    echo "done"
    ```

### Copying data into/out of cluster
We have a transfer node which is a SLURM client.


### Running A Job On Every Node
This is put here as a tool for system administrators needing to do maintenance where a SLURM job is appropriate. Maybe the technique will be useful for someone for a more limited case.

??? note "Click to expand"
    ```
    #!/bin/bash
    #
    # JPHCE - dispatch-to-everynode - Dispatch a job to each node which is responding
    #
    #       FAILS TO WORK IF YOU DON'T SPECIFY A BUNCH OF PARTITIONS
    #       BECAUSE SHARED IS USED. Following worked at this time
    #       #SBATCH --partition=shared,cee,transfer,sysadmin,sas,gpu,bstgpu,neuron
    #
    # You need to specify a batch file at the minimum
    # You can specify additional arguments
    # TODO: Nice to be able to specify a partition to sinfo if desired
    #--------------------------------------------------------------------------
    #--------------------------------------------------------------------------
    # Date          Modification                                       Initials
    #--------------------------------------------------------------------------
    # 20231222      Created, added standard comment section.                JRT
    #--------------------------------------------------------------------------

    usage()
    {
    echo "Usage: $0 [directives..] batchfile "
    echo "Usage:   Specify at least a job file"
    echo "Usage:   Good idea to include in your batch file --output=/dev/null"
    exit 1
    }

    if [ $# -lt 1 ]; then
            usage
    else
            for i in `sinfo -N -r | awk '{print $1}' | sort -u | grep -v NODELIST`
            do
                    echo $i
                    sbatch --nodelist=${i} "$@"
            done
    fi
    ```


## Examples from Elsewhere


### Workflow

[This cluster ](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/WorkflowManagement.html#introduction)has some good material about workflows.

### NERSC Examples
[Good examples](https://docs.nersc.gov/jobs/examples/).

### USC Examples
[Good examples of basic different types of batch jobs](https://www.carc.usc.edu/user-information/user-guides/hpc-basics/slurm-templates)

### Running Multiple Jobs From One Script

[Using srun inside of sbatch scripts,](https://hpc.llnl.gov/banks-jobs/running-jobs/slurm#MultipleJobs) in serial and parallel. Remember to include the `wait` bash command at the end of your batch file so the job doesn't end before all of the tasks inside of it.

### Using signals to clean up/checkpointing

It's a Good Thing to save the state of your computation so that you can pick up where you left off if your job ends earlier than expected.  We should provide some links to existing documentation people have written about how to implement checkpointing.

It's also a Good Thing to clean up after yourself, by, for example, deleting files created in /tmp by your job.

If a job is cancelled or killed because it exceeds its time limit (_maybe_ memory too?), SLURM sends two signals some time apart. Normally the first is a TERM signal, later a KILL signal. You can dispatch jobs with instructions to send them specific signals a specified number of seconds before the KILL signal is sent.

You can modify your batch jobs so they do Good Things when they receive the first signal.

See the [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html) manual page's explanation for the `--signal` argument.

Pay attention to which process(es) are sent signals. The batch job, all of the job steps, ...

Here is a [blog post](https://dhruveshp.com/blog/2021/signal-propagation-on-slurm/) which discusses this in some detail.

That post refers to [this one](https://hpc-discourse.usc.edu/t/signalling-a-job-before-time-limit-is-reached/314/4) which was updated after the post was written, so there might be newer info than was incorporated in the post.

This [stackoverflow answer](https://stackoverflow.com/questions/68214421/graceful-signal-handling-in-slurm/68214873#68214873) seems to take a different approach. This is an advanced topic and will require some care and perhaps experimentation to verify your solution.

