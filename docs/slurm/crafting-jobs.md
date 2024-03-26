---
tags:
  - in-progress
  - slurm
---

# Crafting SLURM Jobs

!!! Note "Authoring Note"
    This document is accumulating information which might best be split into several more-focused documents. There are many aspects of creating jobs.

There are a variety of techniques one can use to initiate SLURM jobs to accomplish various tasks. Here we will describe important topics and accumulate pointers to documentation written by others for their clusters. Later we will write concrete examples of our own.

In the directories under `/jhpce/shared/jhpce/slurm/` you will find files used during orientation, some accumulated documents and batch examples. 

!!! Note
    If you want to submit example batch jobs, or documents describing good workflows for specific tasks, or anything of the kind, we will happily consider adding them to that directory and to this document!!!

## **Sbatch, srun and salloc**

There are three commands used to request resources from SLURM. You will find all three discussed in the [linked documentation](slurm-commands-ref.md).

Here at JHPCE we have been using `srun` primarily as way to start interactive sessions. However it can also be used after allocating resources with `salloc` and inside of `sbatch` scripts. See web site [below](crafting-jobs.md#running-multiple-jobs-from-one-script) for examples.

Be careful using `salloc` that you don't leave allocated resources unused. One way to use salloc is to request resources and launch a new bash shell. Those resources are not released until that bash shell is ended.

## **Sbatch Rules**
1. First characters in batch file need to be: `#!/bin/bash` (although you can use an interpreter other than bash)
2. `#SBATCH` directives need to appear as the first characters on their lines.
3. `#SBATCH` directives need to appear before **_any_** shell commands.
4. You can put comments after # symbols.
4. You may need to add a `wait` command at the bottom to ensure that processes spawned earlier complete before the script does.

## **SLURM Directive Order of Precendence**

!!! Note "Authoring Note"
    This needs to be written. Should it be its own document, for better readability?

For now see these [two PDF pages](images/slurm-precendence.pdf) from an orientation document.

## **Job Environment**
By default jobs start in the same directory that was the current working directory when `sbatch` or `srun` were used. The directive `--chdir=somepath` or `#SBATCH --chdir=somepath` can be used to set the working directory of the job. Whether you choose to use this directive depends on your workflow and typical awareness of where you "are". Would you rather change batch files or set command line arguments to ensure that jobs run in the desired directories or would you rather double-check your location? 

We have a document that describes [other aspects of job environments](../slurm/environment.md).
    
## **Input/output Considerations**

!!! Note "Authoring Note" 
    Use the same terms as used in the storage overview. We want to be consistent. It helps users, and helps us make links between related articles.

- home directory
- project storage
- fastscratch
- local compute node /tmp
    
Some programs have specific variables you can set to indicate where files should be created.

SAS has "WORK" -- is this set to something in the module load process?

R has "TEMPDIR" which defaults to /tmp. We may be changing the module load code to define this to /fastscratch where that is present (which should be everywhere except on C-SUB nodes).

```
You could use your 1TB of fastscratch space for this. So your SLURM script could use commands like:

module load conda_R
export TEMPDIR=$MYSCRATCH
R CMD BATCH myprog.R

```
## **Job Arrays**

Array jobs allow you to use one job script to run many jobs across a set of samples. That SLURM script or the program(s) it calls need(s) to contain syntax that uses SLURM environment variables to assign different work to different task element jobs.

When you submit an array job you can indicate that you only want a certain number of tasks to be executed at a time. This is called the "step size". (You can also use the `scontrol` command to add or adjust that aspect of the job for pending or running jobs. See our [scontrol tips ](../slurm/tips-scontrol.md) page.)

To reference this specific task of a job array, combine SLURM_ARRAY_JOB_ID with SLURM_ARRAY_TASK_ID (e.g. "scontrol update ${SLURM_ARRAY_JOB_ID}_{$SLURM_ARRAY_TASK_ID} ...").

Vendor docs about job arrays are [here](https://slurm.schedmd.com/job_array.html).

New Mexico State University job array section [here](https://hpc.nmsu.edu/discovery/slurm/job-arrays/). Good, compact example document.

[USC explanation](https://www.carc.usc.edu/user-information/user-guides/hpc-basics/slurm-templates) of job arrays.

### **Job array-related environment variables**

These variables are only set if this job is part of a job array.

| Variable name | Meaning |
| -----| ----|
|**SLURM_ARRAY_JOB_ID** | The overall job ID|
|**SLURM_ARRAY_TASK_ID** | The individual task ID|
|**SLURM_ARRAY_TASK_STEP**|The step size of task IDs|
|**SLURM_ARRAY_TASK_MIN**|The minimum task ID|
|**SLURM_ARRAY_TASK_MAX** | The maximum task ID |

## **Dependent jobs**

You can configure jobs to run in order with some conditional control. Conditions include jobs starting, jobs finishing, jobs finishing without errors, all of a set of jobs finishing, etc.

This can be used to achieve different goals, such as

* controlling how many resources you consume at one time when running jobs on PI partitions where there are no enforced QOS limits, and
* breaking up jobs into smaller pieces yet still have them run in the right order. 

Smaller jobs start more quickly, and it can be better to make progress where possible while, for example, waiting for a large amount of RAM to become available on a single node. This is also more efficient for the cluster, since work can be done with, say, less RAM for this portion of the overall task, then a large amount for that portion, then a smaller amount again for some cleanup or merging portion. This is much better than having to wait for a large amount of RAM to become available and then, when it is allocated, tying it down for the entire duration of the single job.

This approach also allows you to use different resources to complete a larger task. For example, use the more common CPU nodes to prepare some data for another job to process on a GPU node, of which there are fewer, and then possibly doing some further data reduction on any available CPU node.

As you can see, people can get more work done more quickly by crafting a set of jobs to use only the necessary resources for each stage of a larger task.

See [this part](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html#OPT_dependency) of the sbatch manual page. Many clusters have sections describing this technique.

## **Heterogeneous Job Support**

These kinds of jobs aim to achieve some of the same goals as the simpler technique of specifying dependencies between jobs. Such as specifying different resource requirements for different components.

Each component of such jobs has virtually all job options available including partition and QOS (Quality Of Service). See this [vendor document](https://slurm.schedmd.com/archive/slurm-22.05.9/heterogeneous_jobs.html).

## **Checkpointing Jobs**

Checkpointing is saving the state of your computation at intervals so that you can pick up where you left off if your job ends earlier than expected.

Losing work is a sad occasion. If you add some complexity to your job scripts you can have them save enough data of the right kind to be able to resume work if the job ends prematurely.  Jobs end through no fault of your own, such as file systems filling up or nodes crashing or needing to be rebooted before they can accept new jobs (this is something that occurs regularly!!!).  If you set a time limit for your job too low, at least you can submit a new job that picks up where the old one left off.

One [example checkpointing document](https://hpc.nmsu.edu/discovery/slurm/backfill-and-checkpoints/#_introduction_to_checkpoint) about implementing this is from the NMSU cluster. It features example code workflows for Python, R and other languages!!!

Here is a Youtube [video about checkpointing from CECI](https://www.youtube.com/watch?v=LyWSYYj4zHY), a European organization.

Until we add more examples, you can search for them yourself.

### **Using signals to clean up/checkpoint**

If a job is cancelled or killed because it exceeds its time limit (_maybe_ also memory too?), SLURM sends two signals some time apart. Normally the first is a TERM signal, later a KILL signal. You can dispatch jobs with instructions to send them specific signals a specified number of seconds before the KILL signal is sent.

You can modify your batch jobs so they do Good Things when they receive the first signal. This extra code is called a *signal handler*.

You can also ask SLURM to send specific signals to your program.

This is an advanced topic and will require some care and perhaps experimentation to verify your solution.

This cluster's [checkpointing](https://hpc-unibe-ch.github.io/slurm/checkpointing.html) page includes examples of handling signals with a focus on checkpointing. Signal handler examples are shown for bash, C/C++ and Python.

See the [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html) manual page's explanation for the `--signal` argument.

Pay attention to which process(es) are sent signals. The batch job, all of the job steps, ...

Here is a [blog post](https://dhruveshp.com/blog/2021/signal-propagation-on-slurm/) which discusses this in some detail.

That post refers to [this one](https://hpc-discourse.usc.edu/t/signalling-a-job-before-time-limit-is-reached/314/4) which was updated after the post was written, so there might be newer info than was incorporated in the post.

This [stackoverflow answer](https://stackoverflow.com/questions/68214421/graceful-signal-handling-in-slurm/68214873#68214873) seems to take a different approach. 

## **Example Batch Jobs**

### **Copying data within cluster**

Here is a sample batch job. More information about this topic is accumulating [here](../files/copying-files.md).

??? note "Click to expand"
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

### **Copying data into/out of cluster**
We have a transfer node which is a SLURM client. It has its own "transfer" partition. You can craft a batch job to move data using that partition by modifying the script in the data copying section above.


### **Running A Job On Every Node**
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


## **Examples from Elsewhere**


### **Workflow**

[This cluster ](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/WorkflowManagement.html#introduction) has some good material about workflows. It includes references to a number of tools which make managing job arrays easier. Also links to papers about the topic of creating workflows for scientific computing.

### **NERSC Examples**
[Good examples](https://docs.nersc.gov/jobs/examples/).

### **USC Examples**
[Good examples of basic different types of batch jobs](https://www.carc.usc.edu/user-information/user-guides/hpc-basics/slurm-templates)

### **Running Multiple Jobs From One Script**

[Using srun inside of sbatch scripts,](https://hpc.llnl.gov/banks-jobs/running-jobs/slurm#MultipleJobs) in serial and parallel. Remember to include the `wait` bash command at the end of your batch file so the job doesn't end before all of the tasks inside of it.



