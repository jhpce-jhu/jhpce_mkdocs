---
tags:
  - in-progress
  - slurm
---

# Crafting SLURM Batch Jobs

!!! Note "Authoring Note"
    This document is accumulating information which might best be split into several more-focused documents. There are many ways of creating jobs!

There are a variety of techniques one can use to initiate SLURM jobs to accomplish various tasks. Here we will describe important topics and accumulate pointers to documentation written by others for their clusters. Later we hope to write concrete examples of our own.

In the directories under `/jhpce/shared/jhpce/slurm/` you will find files used during orientation, some accumulated documents and batch examples. This web site probably contains a better set of information, though.

!!! Note
    If you want to submit example batch jobs, or documents describing good workflows for specific tasks, or anything of the kind, we will happily consider adding them to that directory and to this document!!!

## **Sbatch, srun and salloc**

There are three commands used to request resources from SLURM. You will find all three discussed in the [linked documentation](slurm-commands-ref.md).

Here at JHPCE we have been using `srun` primarily as way to start interactive sessions. However it can also be used after allocating resources with `salloc` and inside of `sbatch` scripts. See web site [below](crafting-jobs.md#running-multiple-jobs-from-one-script) for examples.

Be careful using `salloc` that you don't leave allocated resources unused. One way to use salloc is to request resources and launch a new bash shell. Those resources are not released until that bash shell is ended.

## **Basic Sbatch Rules**
1. First characters in the batch file need to be: `#!/bin/bash` (although you can use an interpreter other than bash)
2. `#SBATCH` directives need to appear as the first characters on their lines.
3. `#SBATCH` directives need to appear before **_any_** shell commands.
4. You can put comments after # symbols.
4. You may need to add a `wait` command at the bottom to ensure that processes spawned earlier complete before the script does.

## **SLURM Directive Order of Precendence**

!!! Note "Authoring Note"
    This needs to be written. Should it be its own document, for better readability?

For now see these [two PDF pages](images/slurm-precendence.pdf) from an orientation document.

## **Job Environment**

### Troubleshooting mysterious failures
The shell that runs your command can have different environments depending on how it was created by the operating system. The "dot files" like `.bashrc` and `.bash_profile` (and equivalents in /etc/) are processed in different combinations depending on whether the shell is a (login or non-login) and (interactive vs non-interactive) one.

This can be extremely confusing! Usually things Just Work, but if you run into problems running batch jobs that you cannot reproduce in interactive debugging sessions, see this important document: [other aspects of job environments](../slurm/environment.md)

### Keep in mind your use of environments via conda or python
You may have dot files which include commands that load named conda environments. If those commands are in the wrong kind of dot file, then they won't be processed during batch jobs.

### Where am I?

By default jobs start in the same directory that was the current working directory where you ran `sbatch` or `srun`. The directive `--chdir=somepath` or `#SBATCH --chdir=somepath` can be used to set the working directory of the job. Whether you choose to use this directive depends on your workflow and typical awareness of where you "are". Would you rather change batch files or set command line arguments to ensure that jobs run in the desired directories or would you rather double-check your location? 

Another file-location issue is where your SLURM output and error files are created. By default they are created by SLURM in the job's working directory. You may want to make them easier to find and/or keep project directories clear of transient clutter by creating directories in your home directory or file allocation (e.g. `/dcs07/my-research-group/data/slurm-output`) and use directives (e.g. `--output=~/slurmout/%j.out`).
    
## **Input/output Considerations**

!!! Note "Authoring Note" 
    Use the same terms as used in the storage overview page. We want to be consistent. It helps users, and helps us make links between related articles.

- home directory
- project storage
- fastscratch
- local compute node /tmp
    
Some programs have specific variables you can set to indicate where files should be created.

SAS has "WORK" -- is this set to something in the module load process?

R has "TMPDIR" which defaults to /tmp. We may be changing the module load code to define this to /fastscratch where that is present (which should be everywhere except on C-SUB nodes).

```
You could use your 1TB of fastscratch space for this. So your SLURM script could use commands like:

module load conda_R
export TMPDIR=$MYSCRATCH
R CMD BATCH myprog.R

```
## **Job Arrays**

!!! Tip
    Excellent set of explanations and examples from [University of Arizona](https://ua-researchcomputing-hpc.github.io/Array-and-Parallel/)!!! (See their [other examples](https://ua-researchcomputing-hpc.github.io))

Array jobs allow you to use one job script to run many jobs across a set of samples. Each array job spawns subordinate "array task" jobs. The overall job has a jobid, and the individual array task jobs are given their own jobid number by appending an underscore and their index number to the master array jobid.

That SLURM script (and the program(s) it calls) need(s) to contain syntax that uses shell and SLURM environment variables to assign different work to different task element jobs. And to store the results in files with unique names.  One has to name files the right way to be able to script the input and output processing.

There are a variety of ways to go about creating such scripts and managing the output.  Example scripts are very useful, so we have provided links to other web sites. There are also tools people have written to help manage the overall workflow. These can, for example, make it easier to identify jobs that failed out an array job and to submit them to run again.

The underscore character is used in commands and is seen in the output of programs like `sacct` as a separator between the job ID number and that of the array task ID number. Output and error files also use underscores in their names.

Controlling your usage with "step size"

When you submit an array job you can indicate that you only want a certain number of tasks to be executed at a time. This will allow other users of your partition to be able to access it, or control the disk and network input/output rate to avoid choking a file server.

You can use the `scontrol` command to add or adjust that aspect of the job for pending or running jobs. See our [scontrol tips ](../slurm/tips-scontrol.md) page.) To reference a specific task of a job array, combine SLURM_ARRAY_JOB_ID with SLURM_ARRAY_TASK_ID (e.g. "scontrol update ${SLURM_ARRAY_JOB_ID}_{$SLURM_ARRAY_TASK_ID} ...").

Vendor docs about job arrays are [here](https://slurm.schedmd.com/job_array.html).

Excellent set of explanations and examples from [University of Arizona](https://ua-researchcomputing-hpc.github.io/Array-and-Parallel/)!!!

New Mexico State University job array section [here](https://hpc.nmsu.edu/discovery/slurm/job-arrays/). Good, compact example document.

Another nice straightforward description is in this [Ronin blog post](https://blog.ronin.cloud/slurm-job-arrays/). 

[USC explanation](https://www.carc.usc.edu/user-information/user-guides/hpc-basics/slurm-templates) of job arrays.

### **Job Array Email Notifications**
 These are handled at the job level, not the individual task element level, _unless_ you provide an extra arguement. Please don't do that, unless you KNOW that your jobs are going to take a long time, so you don't create a mail "storm" of messages.[^1]
 
[^1]: Such storms can get our servers banned by mail administrators, leading to no one getting any job-related email.

### **Job array-related environment variables**

The ++underscore++ARRAY++underscore++ variables are only set if this job is part of a job array.

| Variable name | Meaning | Script variable | Usage | Result |
| -----| ----|-----|-----|----|
|**JOB_ID**|Jobid|%J|#SBATCH %J.err|123.err|
|**SLURM_ARRAY_JOB_ID** | The overall job ID|%A|#SBATCH -o %A.out|123.out|
|**SLURM_ARRAY_TASK_ID** | The individual task ID|%a|#SBATCH -o %A_%a.out|123_6.out||
|**SLURM_ARRAY_TASK_STEP**|The optional step size of task IDs|not available|#SBATCH --array 2-15%3|3|
|**SLURM_ARRAY_INX**|The array index range|not available|#SBATCH --array 2-15|2 15|
|**SLURM_ARRAY_TASK_MIN**|The minimum task ID|not available|#SBATCH --array 2-15|2|
|**SLURM_ARRAY_TASK_MAX** | The maximum task ID |not available|#SBATCH --array 2-15|15|

## **Dependent jobs**

You can configure jobs to run in order with some conditional control. Conditions include jobs starting, jobs finishing, jobs finishing without errors, all of a set of jobs finishing, etc.

This can be used to achieve different goals, such as

* controlling how many resources you consume at one time when running jobs on PI partitions where there are no enforced QOS limits, and
* breaking up jobs into smaller pieces yet still have them run in the right order. For example, copying some data to /fastscratch and, if that was successful, launching an analysis job.

Smaller jobs start more quickly, and it can be better to make progress where possible while, for example, waiting for a large amount of RAM to become available on a single node. This is also more efficient for the cluster, since work can be done with, say, less RAM for this portion of the overall task, then a large amount for that portion, then a smaller amount again for some cleanup or merging portion. This is much better than having to wait for a large amount of RAM to become available and then, when it is allocated, tying it down for the entire duration of the single job.

This approach also allows you to use different resources to complete a larger task. For example, use the more common CPU nodes to prepare some data for another job to process on a GPU node, of which there are fewer, and then possibly doing some further data reduction on any available CPU node.

{==As you can see, people can get more work done more quickly by crafting a set of jobs to use only the necessary resources for each stage of a larger task.==}

You can use different terms (afterany, aftercorr, afterok, afternotok, singleton) and syntaxes to build dependencies. Some people use programs to create dependency graphs to manage their jobs.

!!! Tip
    You may think that the dependency term "after" is okay to use. Note that it does not mean after a job ends. It means after a job is started or cancelled.

Note that SLURM's idea of whether a job completed successfully may not match your definition. You may need to look at the logic of your batch script and add "exit N" statements and other logic to provide non-zero exit codes if something goes wrong before the very last command in the script. You can also add code that specifically checks for success (as opposed to only counting on a final exit code).

See [this part](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html#OPT_dependency) of the sbatch manual page. Many clusters have sections describing this technique. [Yale](https://docs.ycrc.yale.edu/clusters-at-yale/job-scheduling/dependency/). CECI has a bunch of [info on workflows](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/WorkflowManagement.html#introduction) including using additional software to manage job arrays and dependencies.

## **Heterogeneous Job Support**

These kinds of jobs aim to achieve some of the same goals as the simpler technique of specifying dependencies between jobs. Such as specifying different resource requirements for different components.

Each component of such jobs has virtually all job options available including partition and QOS (Quality Of Service). See this [vendor document](https://slurm.schedmd.com/archive/slurm-22.05.9/heterogeneous_jobs.html).

## **Checkpointing Jobs**

Checkpointing is saving the state of your computation at intervals so that you can pick up where you left off if your job ends earlier than expected. And the corresponding logic at the outset of your job that checks for the existence of state data that indicates where it should start and with which files.

Losing work is a sad occasion. If you add some complexity to your job scripts you can have them save enough data of the right kind to be able to resume work if the job ends prematurely.  Jobs end through no fault of your own, such as file systems filling up or nodes crashing or needing to be rebooted before they can accept new jobs (**this is something that occurs regularly!!!**).  If you set a time limit for your job too low, at least you can submit a new job that picks up where the old one left off.

Depending on the workflow you use, adding data saving steps might occur in different locations. Possibilities include:

1. Inside your sbatch script as you loop through some steps -- add a step to save aside checkpoint data, such as which step number you are on and the data needed to resume from that point
2. Inside your sbatch script you might add signal handling code (see section below) if you have a single long-running process. Your code could save aside a copy of generated data with the time and date in its file name.
3. If you have a series of jobs which build upon previous work, you could intersperse checkpointing jobs which save aside state information. You might want to make them dependent upon successful completion of previous ones.

One [example checkpointing document](https://hpc.nmsu.edu/discovery/slurm/backfill-and-checkpoints/#_introduction_to_checkpoint) about implementing this is from the NMSU cluster. {==It features example code workflows for Python, R and other languages!!!==}

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

### **Running Multiple Jobs From One Script**

[Using srun inside of sbatch scripts,](https://hpc.llnl.gov/banks-jobs/running-jobs/slurm#MultipleJobs) in serial and parallel. Remember to include the `wait` bash command at the end of your batch file so the job doesn't end before all of the tasks inside of it. You may need to specify the amount of memory each `srun` command needs (out of the total allocated to the overall job) so the first `srun` isn't given all of the RAM.

### **Workflow**

[This cluster ](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/WorkflowManagement.html#introduction) has some good material about workflows. It includes references to a number of tools which make managing job arrays easier. **Also links to papers about the topic of creating workflows for scientific computing.**

### **NERSC Examples**
[Good examples](https://docs.nersc.gov/jobs/examples/).

### **USC Examples**
[Good examples of basic different types of batch jobs](https://www.carc.usc.edu/user-information/user-guides/hpc-basics/slurm-templates)

### **Using Snakemake**
A workflow-creation tool for Python users called [snakemake](https://snakemake.github.io) can be used to generate SLURM jobs. Here are some links about using snakemake with SLURM:

1. [vendor doc on snakemake and SLURM clusters](https://snakemake.readthedocs.io/en/v7.19.1/executing/cluster.html)
2. a lesson on [implementing snakemake with SLURM](https://www.hpc-carpentry.org/hpc-python/13-cluster/index.html) looks **very** nice for beginners!!! That web page is a part of an overall [short course on doing HPC in Python](https://www.hpc-carpentry.org/hpc-python/)
2. the [BIH cluster's instructions](https://hpc-docs.cubi.bihealth.org/slurm/snakemake/)
3. the [University of Zurich's instructions](https://docs.s3it.uzh.ch/how-to_articles/how_to_run_snakemake/)
4. a [discussion of snakemake](https://news.ycombinator.com/item?id=36735616#) pros and other tools like Nextflow One group talking there developed their own suite called [SciPipe](https://academic.oup.com/gigascience/article/8/5/giz044/5480570)

### **Nextflow**
Use with SLURM is mentioned [here](https://www.nextflow.io/docs/latest/executor.html)

[Nextflow](https://www.nextflow.io/docs/latest/) is a workflow system for creating scalable, portable, and reproducible workflows. It is based on the dataflow programming model, which greatly simplifies the writing of parallel and distributed pipelines, allowing you to focus on the flow of data and computation. Nextflow can deploy workflows on a variety of execution platforms, including your local machine, HPC schedulers, AWS Batch, Azure Batch, Google Cloud Batch, and Kubernetes. Additionally, it supports many ways to manage your software dependencies, including Conda, Spack, Docker, Podman, Singularity, and more.

### **Dead Simple Queue**

[dSQ](https://github.com/ycrc/dsq) from Yale is a set of tools which aim to make running array jobs easier.


`dSQ` itself is a tool that creates batch job scripts for you, and can submit the resulting job. It can create simple jobs but its focus seems to very much be job arrays.

It is written to be used as a module. (Users can create their own modules on JHPCE.)

* You get a simple report of which job ran where and for how long
* [dSQAutopsy](https://github.com/ycrc/dsq?tab=readme-ov-file#dsqautopsy) can create a new job file that has only the jobs that didn't complete from your last run. See [this spot](https://github.com/ycrc/dsq?tab=readme-ov-file#dsq-output) in the README where it describes how to do that.
* All you need is Python 2.7+, or Python 3.



