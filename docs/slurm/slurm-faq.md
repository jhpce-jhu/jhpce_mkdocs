---
tags:
  - slurm
---

# JHPCE SLURM FAQ

The search field on this web site produces good results. Please use it to find answers across all of the documents on the site.

## FAQs from the vendor

[https://slurm.schedmd.com/faq.html](https://slurm.schedmd.com/faq.html)

## FAQs from other clusters
Note that the information in these pages will include details which do not apply here in JHPCE. Caveat emptor.

[CECI](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/SlurmFAQ.html)

## Where can I learn more about SLURM-related commands?
Start with [this document](../slurm/slurm-commands-ref.md) which describes commands and provides links to more information. Commands documented include both those written by SLURM's vendor, but also ones we have created.

## What are the default job resources?

??? "Click to open"
    Jobs that do not specify CPUs, RAM, or duration, or partition receive 1 CPU, 5G of RAM, assignment to the `shared` partition, and, because shared is a [public partition](../slurm/partitions.md#public-partitions), a [time limit](../slurm/time-limits.md) of 1 day. 

## When will my job start?

??? "Click to open"
    See [this document](whenstart.md) for a description of factors affecting when jobs start.  Of course the load on the cluster impacts job start times. Please consult the output of `slurmpic` for information about the state of the cluster and its available resources. Note that your job cannot start until a match is found for the resources you specified. There may be a lot of unused CPUs on a node, for example, but if someone has allocated all of the RAM on that node your job won't fit there.
    
    If the Reason given for a pending job is `QOSMaxMemoryPerUser` or `QOSMaxCpuPerUserLimit` then you can read about Quality of Service in our document [here](../slurm/qos.md).
    
## How will I know if my job ended?
See [this document](monitoring.md) for information about monitoring pending, running and completed jobs. You can specify directives requesting email notfication when jobs start, fail, etc.


## Help! My job is going to run out of time!

??? "Click to open"
    You can modify the time limit of a pending job, but not one that has begun running.
    You can email bitsupport and ask system administrators to extend the time limit of running jobs. Please provide the job id number(s) and the new, revised total duration.
    You can add "mailtype" and "mailuser" directives when submitting a job, while it is pending, and while it is running. See [this document](../slurm/time-limits.md) for instructions.
    
## What partitions exist?

??? "Click to open"
    The default partition is "shared". By default `slurmpic` describes the state of this partition. (Run `slurmpic -h` to see a list of options, including the flag to show other partitions.)

    See [this document](partitions.md) for a description of partitions and their purposes and limits.

## How do I cancel a job?

??? "Click to open"
    Use `scancel <jobid>` where jobid is the number for your job. You can specify multiple jobs in a space-separated list.

    If your job is a member of a job array, it will have an underscore in it, e.g. 2095_15. You can cancel an array task (2095_15) or the whole job array (2095).

    If you want to cancel all of your pending jobs, use `scancel --me -t PENDING`. If you want to cancel all of your jobs, use `scancel -u your-username`

## How do I hold or modify a job?

??? "Click to open"
    Note that if you want to modify something about the job, instead of cancelling it and losing any accumulated [age priority](whenstart.md#priority) it has accrued, you can hold the job, modify it and release it with `scontrol` commands. Only some of the parameters of a job can be modified when it is pending.

    `scontrol hold <jobid>` where <jobid> can be a comma-separated list.

    If your jobs have names, you can hold using the name. (This will hold all matching jobs.) `scontrol hold jobname=<jobname>` `scontrol update jobid=<jobid> ...` See [this section](https://slurm.schedmd.com/scontrol.html#lbAH) of the manual page for a list of attributes which can be modified.

    We have a number of [scontrol examples](tips-scontrol.md) spelled out for you.
    
    You can submit a job directly into a held status if you want to delay its start, using the `-H` or `--hold` directive. However, you probably want to instead use the `-b` or `--begin` directive to specify a start time. Or, if you want a job to wait until something else finishes, you can make it dependent on the first job. See [this document](../slurm/crafting-jobs.md/#dependent-jobs) for information about dependent jobs.

## How do I control the order my jobs start?

??? "Click to open"
    You can rank your jobs with the "nice" and "top" subcommands to `scontrol`.  See the `scontrol` [tips document](tips-scontrol.md).
    You can also submit jobs with [dependency directives](https://slurm.schedmd.com/sbatch.html#OPT_dependency) and also create [heterogenous jobs](https://slurm.schedmd.com/heterogeneous_jobs.html) which spawn other jobs. See [our document](../slurm/crafting-jobs.md/#dependent-jobs) for information about dependent jobs.

## "Unable to allocate resources"

??? "Click to open"
    If the scheduler determines that your job is invalid in some fashion, it will generally reject it immediately instead of putting it into the queue with a pending status. There are a few causes of this. The wording of the error may or may not be clear. Inspect your job directives. Are you asking for more resources than any node can ever provide?
    
## "srun: error: Something is wrong with the boot of the nodes."

??? "Click to open"
    If you see this message, it is likely that the allocated compute node's /tmp/ file system is full. Please cancel your job (scancel -j <jobid>), resubmit it excluding that particular node (add to the job's arguments --exclude=compute-<the-right-number-for-you>), and send an email to bitsupport@lists.jh.edu mentioning the jobid, node involved, and that you received this error messsage.
    
## How to run your job on the fastest node

??? "Click to open"
    JHPCE keeps nodes running until they die. Therefore there is a wide variety of machine configurations, including the kinds of CPUs and the speed of data buses between the CPU and other components such as memory. As an example, our oldest machines use [DDR3 RAM](https://en.wikipedia.org/wiki/DDR3_SDRAM) while our latest use [DDR5 RAM](https://en.wikipedia.org/wiki/DDR5_SDRAM) (our oldest node has RAM configured to run at 1333 MT/s versus 4400 MT/s on our very newest node). Generally speaking, the later the machine the faster a job (especially those which are CPU-bound (don't do a lot of network I/O)) job can run.  (Of course the load on a node from other people's jobs will impact the performance of your job.) We have defined a SLURM parameter for each node which lists its "features"  You can specify that your job _prefers_ or _requires_ certain features. (Of course doing that means that it may take longer for your job to start.)[Our page describing how to do this](../slurm/node-features.md) includes a list of CPU model _family names_ as used/defined by the GNU compiler, in alphabetical order.
    
    Similarly, you can specify that your job only be allowed to run on specific node or nodes by issuing the `--nodelist=compute-blah1,compute-blah2]` directive to your `srun` or `sbatch` commands.  You can also provide a file name to specify a file which contains a list of nodes. See the nodelist info in the [sbatch manual page](https://slurm.schedmd.com/sbatch.html).


## User's group not permitted to use this partition

??? "Click to open"
    Some of our PI partitions have UNIX groups defined to control who can submit jobs to them. If you are not a member of that group and try to submit a job, you'll see an error like this:

    `srun: error: Unable to allocate resources: User's group not permitted to use this partition`

    You can see which groups you belong to with the `groups` command.

    You can see which group you need to belong to by looking for the partition in question in the file `/etc/slurm/partitions.conf`
    
    If you feel like you were not included in a group in error, ask the PI to contact bitsupport@lists.jh.edu asking us to add you. 

## Job violates accounting/QOS policy

??? "Click to open"
    If you ask for more resources than will ever be able to be allocated to you, you might receive one of several error messages.

    This one appeared when a job asked for more RAM than was allowed by a QOS limit:

    `Unable to allocate resources: Job violates accounting/QOS policy (job submit limit, user's size and/or time limits)`

    But when more CPUs were requested (instead of too much RAM), the error was different:

    `Unable to allocate resources: More processors requested than permitted`

## My job won't run on a particular node

??? "Click to open"
    If a node is misconfigured and your job runs on other nodes, then please report the problem to bitsupport@lists.jh.edu (see [this document](../help/good-query.md)). In the meantime, you can exclude that node from being considered to run your job by supplying the `--exclude=nodename1,nodename2` directive to your `srun` or `sbatch` commands.  You can also provide a file name to specify a file which contains a list of nodes. See the exclude info in the [`sbatch` manual page](https://slurm.schedmd.com/sbatch.html)

## How many jobs can I run at a time?

??? "Click to open"
    As of 20240405 we are limiting the number of simultaneous jobs submitted or running to 10,000.

    Array jobs are also limited to 10,000 tasks by variable `max_array_tasks` in /etc/slurm/slurm.conf

    Total jobs at a single time in the whole cluster is 90,000, which is determined by the variable `MaxJobCount` in /etc/slurm/slurm.conf

## The cluster is slower than my laptop/desktop!!

??? "Click to open"
    From a purely hardware perspective, this is unlikely. However, it might be true because you ARE using shared and distributed resources. For example, data files are being stored on file servers and used by compute nodes over a network. We use RAID on our file servers to distribute the input/output load over many hard drives, which is usually  faster than what a single hard drive can provide.  We use 40 gigabit-per-second (or faster) network interfaces on our file servers, and 10 gigabit-per-second network interfaces on our compute nodes. But everything your desktop or laptop is doing is local, and dedicated only to you.
    
    If your job is running more slowly than you expect, the most common reasons are:

    - Incorrect/inadequate resource request (you only requested 1 CPU core, or the default amount of RAM)
    - Your code is doing something that you don't know about

    A combination of these two factors are almost always the reason for jobs running more slowly on the cluster than on a different resource.

    One example is Matlab code running on the cluster versus a modern laptop. Matlab implicitly performs some operations in parallel (e.g. matrix operations), spawning multiple threads to accomplish this parallelization. If your resource request on the cluster only includes 1 or 2 CPU cores, no matter how many threads Matlab spawns, they will be pinned to the cores you were assigned.

    This can lead to a situation where a user's Matlab code is running using all 4 CPU cores on their laptop, but only 1 CPU core on the cluster. This can give the impression that the cluster is slower than a laptop. By requesting 4 or 8 CPU cores in your resource request, Matlab can now run these operations in parallel.

    It is important to understand what the code or application you are using is actually doing when you run your job

## X11-related errors

See also our [general X11 document](../access/x11.md) for other information.

### Can't open display

??? "Click to open"
    If you are on a compute node in an interactive session and get a message like `Error: Can't open display: localhost:12.0` when launching an X11 program, something went wrong while other things went okay. (Normally if there is an X11 problem you don't wind up having a DISPLAY variable set.) So far this error has appeared after an `srun: error: _half_duplex: read error -1 Connection reset by peer` error. See that entry in this FAQ.

### srun: error: _half_duplex

??? "Click to open"
    `srun: error: _half_duplex: read error -1 Connection reset by peer`

    We consider this a SLURM bug in version 22.05.09. It appears when `srun` is used with the `--x11` argument. Sometimes immediately, but typically when an X11 program is launched.

    If you do not intend to run any X11 programs during your interactive session, then you can log out of that session and start a new one without the `--x11` flag.

    If you do intend to use X11 programs, when that error appears the only solution we have found is to *abandon* that whole interactive session to the compute node by typing “exit” to quit the shell. Back on the login node, verify that basic X11 functionality works by starting a simple X11 command, such as the `xterm` or `xclock` programs. If that works, start a new srun command to get back onto a compute node. Once on a compute node, verify that basic X11 functionality works by starting either the xterm or xclock programs. If they did, then try your X11 program again.

### srun: error: Ignoring --x11 option
??? "Click to open"
    `srun: error: Ignoring --x11 option for a job step within an existing job. Set x11 options at job allocation time.`
    
    This error is issued because the srun command was run after already logging into a compute node via an interactive job. In other words, it is an srun inside of an srun.  `Srun` can be issued inside of *resource allocations* created by the `salloc` or `sbatch commands`, but not inside of other srun’s.

### srun-related error: Could not retrieve magic cookie

??? "Click to open"
    `Could not retrieve magic cookie. Cannot use X11 forwarding`
    
    This error is issued because the srun command was run after already logging into a compute node via an interactive job. In other words, it is an srun inside of an srun.  `Srun` can be issued inside of *resource allocations* created by the `salloc` or `sbatch commands`, but not inside of other srun’s.
