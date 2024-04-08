---
tags:
   - slurm
---
# SLURM COMMANDS
Here are links to online copies of the manual pages for commands. If we've
written a page with advice about using the command, use the {==(LOCAL TIPS)==} link.

## Locally Written Tools
* **slurmpic**: An **essential** program for getting cluster status info. Use -h option to see essential usage details. (no man page yet)
* **slurmuser**: Displays a per-user set of details about what the running jobs are using in terms of cores and RAM. Like most commands we create, using the “-h” argument will reveal useful usage information.
* **smem**: Displays memory used by your currently running jobs. If given a jobid number, it will display info about the memory usage of that job. (no man page yet)
* memory reporting script - puts per-user output daily into directories under /jhpce/shared/jhpce/jhpce-log/

## Contributed Programs We've Installed
* **seff**: Display efficiency of CPU and RAM usage of a completed job. (no man page yet)
* **[slurm-mail:](https://github.com/neilmunday/slurm-mail)** Tool used to add details to mail sent to you. Not something you can modify. Listed for completeness.
 
## Provided with Slurm

All of the manual pages are [here](https://slurm.schedmd.com/archive/slurm-22.05.9/man_index.html), including those for the configuration files found in /etc/slurm/

### Submitting Jobs


* [salloc](https://slurm.schedmd.com/archive/slurm-22.05.9/salloc.html): request an interactive job allocation (doesn't start any processes anywhere)
* [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html): submit a batch script to Slurm to create an allocation and run processes
* [srun](https://slurm.schedmd.com/archive/slurm-22.05.9/srun.html): launch one or more tasks of an application using allocated resources

### Information about cluster and jobs
!!! Warning
    **Do not frequently[^1] run slurmpic, squeue, sacct or other Slurm client commands using loops in shell scripts or other programs.** 

    These commands all send remote procedure calls to slurmctld, the main SLURM control and scheduling daemon, They may also perform look-ups in the accounting database. That process and the database need to be *highly responsive* to the input/output caused by running jobs.

    Ensure that programs limit calls to slurmctld to the minimum necessary for the information you are trying to gather. Add arguments to limit to needed partitions or users or job data fields, etcetera.

[^1]: Frequently meaning more than once every **five minutes**. Do you REALLY **need** to know something sooner than that? If you want to know when a job starts, fails, or finishes, use email notification settings. You can add them to pending and running jobs using [scontrol](../slurm/tips-scontrol.md). (See sbatch manual page for possible mail types.)

Some SLURM commands such as sacct and squeue can display a wide variety of information. It can be complex to specify what you want to see and to format it so it is readable. We've tried to document some common choices in the LOCAL TIPS documents. A tip: you  set certain environment variables to specify output arguments instead of providing the arguments on the command line. It can be useful to define these different ways in aliases or shell scripts to format output in ways you need, because simply changing the value of these variables can produce vastly different output for commands like sacct and squeue. Example variables are: SLURM_TIME_FORMAT, SACCT_FORMAT, SQUEUE_FORMAT, SQUEUE_FORMAT2, SQUEUE_SORT. 

* [sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html): {==([LOCAL TIPS](tips-sacct.md))==}: display accounting data for jobs in the Slurm database
* [sattach](https://slurm.schedmd.com/archive/slurm-22.05.9/sattach.html): attach to a running job step
* [scontrol](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html): {==([LOCAL TIPS](tips-scontrol.md))==}: display (or modify when permitted) the status of Slurm entities (jobs, nodes, partitions, reservations)
* [sinfo](https://slurm.schedmd.com/archive/slurm-22.05.9/sinfo.html): display node and partition information
* [sprio](https://slurm.schedmd.com/archive/slurm-22.05.9/sprio.html): {==([LOCAL TIPS](whenstart.md/#priority))==}: display the factors that comprise a job's scheduling priority
* [squeue](https://slurm.schedmd.com/archive/slurm-22.05.9/squeue.html): display the jobs in the scheduling queues, one job per line
* [sshare](https://slurm.schedmd.com/archive/slurm-22.05.9/sshare.html): display the shares and usage for each charge account and user
* [sstat](https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html): display process statistics of a running job/step
* [sview](https://slurm.schedmd.com/archive/slurm-22.05.9/sview.html): X11 graphical tool for displaying jobs, partitions, reservations


### Controlling Jobs
* [scancel](https://slurm.schedmd.com/archive/slurm-22.05.9/scancel.html): cancel or pause a job or job step or signal a running job or job step to pause
* [scontrol](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html): {==([LOCAL TIPS](tips-scontrol.md))==}: display (and modify when permitted) the status of Slurm entities (jobs, nodes, partitions, reservations)

### For Systems Administrators
* [sacctmgr](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html):
* [scontrol](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html): {==([LOCAL TIPS](tips-sacctmgr.md))==}: display and modify Slurm account information
* [sdiag](https://slurm.schedmd.com/archive/slurm-22.05.9/sdiag.html): display scheduling statistics and timing parameters
* [slurmctld](https://slurm.schedmd.com/archive/slurm-22.05.9/slurmctld.html): central management daemon
* [slurmd](https://slurm.schedmd.com/archive/slurm-22.05.9/slurmd.html): client-side daemon
* [sreport](https://slurm.schedmd.com/archive/slurm-22.05.9/sreport.html): generate canned reports from job accounting data and machine utilization statistics


