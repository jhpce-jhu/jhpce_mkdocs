# SLURM COMMANDS
Here are links to online copies of the manual pages for commands. If we've
written a page with advice about using the command, use the {==(LOCAL TIPS)==} link.

## Locally Written Tools
slurmpic: Essential program for getting cluster status info. Use -h option to see essential usage details. (no man page yet)

## Contributed Programs
* seff: Display efficiency of CPU and RAM usage of a completed job. (no man page yet)
* [slurm-mail:](https://github.com/neilmunday/slurm-mail) Tool used to add details to mail sent to you. Not something you can modify. Listed for completeness.
 
## Provided with Slurm

All of the manual pages are [here](https://slurm.schedmd.com/archive/slurm-22.05.9/man_index.html), including those for the configuration files found in /etc/slurm/

### Submitting Jobs
* [salloc](https://slurm.schedmd.com/archive/slurm-22.05.9/salloc.html): request an interactive job allocation
* [sattach](https://slurm.schedmd.com/archive/slurm-22.05.9/sattach.html): attach to a running job step
* [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html): submit a batch script to Slurm
* [srun](https://slurm.schedmd.com/archive/slurm-22.05.9/srun.html): launch one or more tasks of an application across requested resources

### Information about cluster and jobs
Do not run slurmpic, squeue, sacct or other Slurm client commands that send remote procedure calls to slurmctld, the main SLURM control and scheduling daemon, from loops in shell scripts or other programs. Ensure that programs limit calls to slurmctld to the minimum necessary for the information you are trying to gather.

* [sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html): {==([LOCAL TIPS](tips-sacct.md))==}: display accounting data for jobs in the Slurm database
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


