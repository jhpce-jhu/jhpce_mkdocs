---
tags:
  - in-progress
  - slurm
---

# Crafting SLURM Jobs

There are a variety of techniques one can use to initiate SLURM jobs to accomplish various tasks. Here we will initially accumulate pointers to documentation written by others for their clusters. Later we will write concrete examples of our own.

## Sbatch, srun and salloc

There are three commands used to request resources from SLURM. You will find all three discussed in the linked documentation.

Here at JHPCE we have been using srun primarily as way to start interactive sessions.


## Workflow

[This cluster ](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/WorkflowManagement.html#introduction)has some good material about workflows.

## NERSC Examples
[Good examples](https://docs.nersc.gov/jobs/examples/).

## Running Multiple Jobs From One Script

[Using srun inside of sbatch scripts,](https://hpc.llnl.gov/banks-jobs/running-jobs/slurm#MultipleJobs) in serial and parallel. Remember to include the `wait` bash command at the end of your batch file so the job doesn't end before all of the tasks inside of it.

## Dependent jobs

You can configure jobs to run in order with some conditional control. See [this part](https://slurm.schedmd.com/sbatch.html#OPT_dependency) of the sbatch manual page.

## Heterogeneous Job Support

Each component of such jobs has virtually all job options available including partition, account and QOS (Quality Of Service). See this [vendor document](https://slurm.schedmd.com/heterogeneous_jobs.html).

## Copying data within cluster

Here is a sample batch job. More information about this topic is accumulating [here](../files/copying-files.md).

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

## Copying data into/out of cluster
