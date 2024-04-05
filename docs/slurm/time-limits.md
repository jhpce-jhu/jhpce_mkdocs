---
tags:
  - in-progress
  - slurm
---

# Time Limits

```
Note: The following was largely written using ChatGPT.
```

## Setting a time limit for your SLURM job on JHPCE

One crucial aspect of job management in SLURM is setting time limits for job execution. Time limits help ensure fair resource allocation, prevent jobs from monopolizing system resources, and facilitate efficient utilization of computing resources.

## Importance of Time Limits:
Setting time limits is essential for maintaining a balanced workload on an HPC system. By specifying a maximum runtime for each job, users prevent inefficient use of resources and minimize delays for others waiting to run their tasks. Time limits also aid in preventing runaway or stuck jobs that may consume resources indefinitely. This proactive approach to job management promotes fairness and improves the overall system efficiency, making it more accessible to a larger number of users.

## Syntax for Setting Time Limits:
In SLURM, users can set time limits using the --time or -t option when submitting a job. 

The time limit is specified in the format **days-hours:minutes:seconds**. For example, to request a job with a maximum runtime of 2 days, the following command can be used:

`sbatch --time=2-00:00:00 my_script.sh`

Or simply

`sbatch --time=2-00 myscript.sh`


To request an srun session with at 4 hour time limit you would run:

`srun --pty --x11 -t 4:00:00 bash`

Note that the “-t” option should be followed by a space and the “–time” option should be followed by an “=”. This syntax allows users to tailor the time limit to the specific requirements of their jobs, balancing the need for computational resources with fair usage practices.

## JHPCE Cluster Time Limit Policies:
On the JHPCE cluster, specific time limit policies are in place to balance the needs of diverse users. The default time limit for job execution on the [public partitions](../slurm/partitions.md) (shared, interactive and gpu partitions) is set to 1 day, ensuring that shorter tasks do not face unnecessary delays. 

Additionally, users have the flexibility to request a maximum time limit of up to 90 days for longer-running jobs. This range accommodates a variety of computational workloads, offering users the freedom to tailor time limits according to the requirements of their specific tasks. Please note that by default these limits are only set on the [public partitions](../slurm/partitions.md) (shared, interactive and gpu partitions).

## Best Practices:

### Receive Email Notifications

The directives `--mail-user` and `--mail-type` are used to tell SLURM that you want to receive email updates for certain events. One set of events is related to an approaching time limit: TIME_LIMIT_50, TIME_LIMIT_80, TIME_LIMIT_90 and TIME_LIMIT.

If you did not use these directives when submitting your job, you can define or modify them on both pending and running jobs using `scontrol`

```Shell title="Be notified at 80% of job duration" linenums="0"
scontrol update jobid=<jobid> mailtype=time_limit_80
```
```Shell title="But only if you tell it where to send email" linenums="0"
scontrol update jobid=<jobid> mailuser=<your-address@jh.edu>
```


### What ChatGPT Wrote:

When setting time limits in SLURM, it is crucial to consider the nature of the computational task.
Jobs with well-defined execution times benefit from precise time limits, while more unpredictable tasks may require a degree of flexibility.
Regularly reviewing and adjusting time limits based on job characteristics and system performance is a best practice to ensure optimal utilization of resources.
If you don’t know precisely how long your job will take to run, it is generally better to request a longer time limit than expected, to avoid jobs being terminated prematurely mid-run.
The time limit on an array job will be applied to each task, rather than the job as a whole.
You can tell that a job has been terminated due to exceeding its time limit by noting that the “State” in the output of “sacct” is “TIMEOUT”, or noting the a line like “slurmstepd: error: *** JOB 123456 ON compute-123 CANCELLED AT 2024-02-13T12:03:27 DUE TO TIME LIMIT ***” in the SLURM output file.

```
[mmill116@login31 class-scripts]$ cat script1
 #!/bin/bash
 # Test script for JHPCE cluster
 date
 echo "I am now running on compute node:"
 hostname
 SLEEPTIME=$((20 + $RANDOM % 60))
 echo "Sleeping " $SLEEPTIME " seconds..."
 sleep $SLEEPTIME
 date
 echo "Done..."
 exit 0 
[mmill116@login31 class-scripts]$ sbatch -a 1-10%4 -t 00:00:30 script1
Submitted batch job 1980474 

[mmill116@login31 class-scripts]$ sacct -j 1980474
 JobID           JobName  Partition    Account  AllocCPUS      State ExitCode 
 ------------ ---------- ---------- ---------- ---------- ---------- -------- 
 1980474_1       script1     shared      jhpce          2  COMPLETED      0:0 
 1980474_1.b+      batch                 jhpce          2  COMPLETED      0:0 
 1980474_1.e+     extern                 jhpce          2  COMPLETED      0:0 
 1980474_2       script1     shared      jhpce          2    TIMEOUT      0:0 
 1980474_2.b+      batch                 jhpce          2  CANCELLED     0:15

. . .

[mmill116@login31 class-scripts]$ cat slurm-1980474_2.out
Tue Feb 13 12:02:19 PM EST 2024
I am now running on compute node:
compute-093.cm.cluster
Sleeping  71  seconds...
slurmstepd: error: *** JOB 1980476 ON compute-093 
CANCELLED AT 2024-02-13T12:03:27 DUE TO TIME LIMIT *** 
```

## Backfill Scheduler Strategies:

SLURM’s backfill scheduler introduces an additional layer of sophistication to job scheduling. Backfilling allows shorter jobs to be scheduled into available resources ahead of longer jobs, provided that the overall system efficiency is not compromised. Time limits play a crucial role in backfill strategies, ensuring that shorter jobs do not overstay their welcome while maximizing resource utilization. The backfill scheduler contributes to improved job throughput, reduced queue times, and enhanced overall system responsiveness.

## Conclusion:
In conclusion, setting time limits in SLURM is a fundamental practice for efficient job management in HPC environments. It promotes fair resource allocation, prevents runaway jobs, and contributes to the overall effectiveness of the computing cluster. Users should be mindful of their job requirements, system dynamics, and best practices to harness the full potential of SLURM’s capabilities while maintaining a balanced and responsive computing environment.

