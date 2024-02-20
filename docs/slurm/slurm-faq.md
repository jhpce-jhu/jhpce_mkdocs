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

Use `scancel jobid` where jobid is the number for your job.
If your job is a member of a job array, it will have an underscore in it, e.g. 2095_15. You can cancel an array task or the whole job array. 

## Unable to allocate resources
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
