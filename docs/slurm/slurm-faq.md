---
tags:
  - slurm
---

# JHPCE SLURM FAQ

Use the search field if desired.

## FAQs from other clusters
Note that the information in these pages will include details which do not apply here in JHPCE. Caveat emptor.

[CECI](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/SlurmFAQ.html)


## When will my job start?

??? "Click to open"
    See [this document](whenstart.md) for a description of factors affecting when jobs start.  Of course the load on the cluster impacts job start times. Please consult the output of `slurmpic` for information about the state of the cluster and its available resources. Note that your job cannot start until a match is found for the resources you specified. There may be a lot of unused CPUs on a node, for example, but if someone has allocated all of the RAM on that node your job won't fit there.
    
    If the Reason given for a pending job is `QOSMaxMemoryPerUser` or `QOSMaxCpuPerUserLimit` then you can read about Quality of Service in our document [here](../slurm/qos.md).

## What partitions exist?

??? "Click to open"
    The default partition is "shared". By default `slurmpic` describes the state of this partition. (Run `slurmpic -h` to see a list of options, including the flag to show other partitions.)

    See [this document](partitions.md) for a description of partitions and their purposes and limits.

## How will I know if my job ended?
See [this document](monitoring.md) for information about monitoring pending, running and completed jobs.

## How do I cancel a job?

??? "Click to open"
    Use `scancel <jobid>` where jobid is the number for your job. You can specify multiple jobs in a space-separated list.

    If your job is a member of a job array, it will have an underscore in it, e.g. 2095_15. You can cancel an array task (2095_15) or the whole job array (2095).

    If you want to cancel all of your pending jobs, use `scancel --me -t PENDING`. If you want to cancel all of your jobs, use `scancel -u your-username`

## How do I hold or modify a job?

??? "Click to open"
    Note that if you want to modify something about the job, instead of cancelling it and losing any accumulated [age priority](whenstart.md#priority) it has accrued, you can hold the job, modify it and release it with `scontrol` commands. Only some of the parameters of a job can be modified when it is pending, and even fewer if it has started running.

    `scontrol hold <jobid>` where <jobid> can be a comma-separated list.

    If your jobs have names, you can hold using the name. (This will hold all matching jobs.) `scontrol hold jobname=<jobname>` `scontrol update jobid=<jobid> ...` See [this section](https://slurm.schedmd.com/scontrol.html#lbAH) of the manual page for a list of attributes which can be modified.

    We have a number of [scontrol examples](tips-scontrol.md) spelled out for you.

## How do I control the order my jobs start?

??? "Click to open"
    You can rank your jobs with the "nice" and "top" subcommands to `scontrol`.  See the `scontrol` tips document mentioned above.
    You can also submit jobs with [dependency directives](https://slurm.schedmd.com/sbatch.html#OPT_dependency) and also create [heterogenous jobs](https://slurm.schedmd.com/heterogeneous_jobs.html) which spawn other jobs. 

## "Unable to allocate resources"

??? "Click to open"
    If the scheduler determines that your job is invalid in some fashion, it will generally reject it immediately instead of putting it into the queue with a pending status. There are a few causes of this. The wording of the error may or may not be clear.

### User's group not permitted to use this partition

??? "Click to open"
    Some of our PI partitions have UNIX groups defined to control who can submit jobs to them. If you are not a member of that group and try to submit a job, you'll see an error like this:

    `srun: error: Unable to allocate resources: User's group not permitted to use this partition`

    You can see which groups you belong to with the `groups` command.

    You can see which group you need to belong to by looking for the partition in question in the file /etc/slurm/partitions.conf

### Job violates accounting/QOS policy

??? "Click to open"
    If you ask for more resources than will ever be able to be allocated to you, you might receive one of several error messages.

    This one appeared when a job asked for more RAM than was allowed by a QOS limit:

    `Unable to allocate resources: Job violates accounting/QOS policy (job submit limit, user's size and/or time limits)`

    But when more CPUs were requested (instead of too much RAM), the error was different:

    `Unable to allocate resources: More processors requested than permitted`

## How many jobs can I run at a time?

??? "Click to open"
    As of 20240220 there are few limits. We are not currently limiting the number of simultaneous jobs submitted or running.

    Array jobs are limited to 15,000 tasks by variable `max_array_tasks` in /etc/slurm/slurm.conf

    Total jobs at a single time is 90,000, which is determined by the variable `MaxJobCount` in /etc/slurm/slurm.conf

## srun: error: _half_duplex

??? "Click to open"
    `srun: error: _half_duplex: read error -1 Connection reset by peer`

    We consider this a SLURM bug. It appears when `srun` is used with the `--x11` argument. Sometimes immediately, but typically when an X11 program is launched.

    If you do not intend to run any X11 programs during your interactive session, then you can log out of that session and start a new one without the `--x11` flag.

    If you do intend to use X11 programs, when that error appears the only solution we have found is to abandon that whole login session to the compute node by typing “exit” to quit the shell. Back on the login node, verify that basic X11 functionality works by starting either the xterm or xclock programs. If that works, start a new srun command to get back onto a compute node. Once on a compute node, verify that basic X11 functionality works by starting either the xterm or xclock programs. If they did, then try your X11 program again.

## srun: error: Ignoring --x11 option
??? "Click to open"
    `srun: error: Ignoring --x11 option for a job step within an existing job. Set x11 options at job allocation time.`
    
    This error is issued because the srun command was run after already logging into a compute node via an interactive job. In other words, it is an srun inside of an srun.  `Srun` can be issued inside of *resource allocations* created by the `salloc` or `sbatch commands`, but not inside of other srun’s.
