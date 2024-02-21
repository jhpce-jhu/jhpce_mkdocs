---
tags:
  - in-progress
---

# JHPCE SLURM FAQ

Use the search field if desired.

[Example document](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/SlurmFAQ.html)


## When will my job start?
See [this document](whenstart.md). 

## What partitions exist?
See [this document](partitions.md).

## How do I cancel a job?

Use `scancel <jobid>` where jobid is the number for your job. You can specify multiple jobs in a space-separated list.

If your job is a member of a job array, it will have an underscore in it, e.g. 2095_15. You can cancel an array task (2095_15) or the whole job array (2095).

If you want to cancel all of your pending jobs, use `scancel --me -t PENDING`. If you want to cancel all of your jobs, use `scancel -u your-username`

## How do I hold or modify a job?
Note that if you want to modify something about the job, instead of cancelling it and losing any accumulated age priority it has accrued, you can hold the job and modify it with `scontrol` commands. Only some of ther parameters of a job can be modified when it is pending, and even fewer if it has started running.

`scontrol hold <jobid>` where <jobid> can be a comma-separated list.
If your jobs have names, you can hold using the name. (This will hold all matching jobs.) `scontrol hold jobname=<jobname>`
`scontrol update jobid=<jobid> ...` See [this section](https://slurm.schedmd.com/scontrol.html#lbAH) of the manual page for a list of attributes which can be modified.

## "Unable to allocate resources"
If the scheduler determines that your job is invalid in some fashion, it will generally reject it immediately instead of putting it into the queue with a pending status. There are a few causes of this. The wording of the error may or may not be clear.

### User's group not permitted to use this partition
Some of our PI partitions have UNIX groups defined to control who can submit jobs to them. If you are not a member of that group and try to submit a job, you'll see an error like this:

`srun: error: Unable to allocate resources: User's group not permitted to use this partition`

You can see which groups you belong to with the `groups` command.

You can see which group you need to belong to by looking for the partition in question in the file /etc/slurm/partitions.conf

### Job violates accounting/QOS policy
If you ask for more resources than will ever be able to be allocated to you, you might receive one of several error messages.

This one appeared when a job asked for more RAM than was allowed by a QOS limit:

`Unable to allocate resources: Job violates accounting/QOS policy (job submit limit, user's size and/or time limits)`

But when more CPUs were requested (instead of too much RAM), the error was different:

`Unable to allocate resources: More processors requested than permitted`

## How many jobs can I run at a time?

As of 20240220 there are few limits.

Array jobs are limited to 15,000 tasks by variable `max_array_tasks` in /etc/slurm/slurm.conf

Total jobs at a single time is 90,000, which is determined by the variable `MaxJobCount` in /etc/slurm/slurm.conf
