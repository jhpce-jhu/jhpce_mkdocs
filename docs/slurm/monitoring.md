---
tags:
  - in-progress
  - slurm
---

# Monitoring SLURM Jobs

Has my job ended? Why did my job fail? How much memory did my sample job use so I can use memory efficiently in for future similar jobs?

There are a number of ways in which you can learn the answers to these questions.

Making sure your jobs request the right amount of RAM and the right number of CPUs helps you and others using the clusters use these resources more effeciently, and in turn get work done more quickly. Below are some examples of how to measure your CPU and RAM (aka memory) usage so you can make this happen.

## Email Notification

You can direct SLURM to send you email when various things happen with your job. These directives can be given on the command line, in batch job scripts, and set in your SLURM defaults file. You can even modify running jobs to set or change their notification settings (see the [scontrol tips](tips-scontrol.md) page).

!!! Warning
    Take care not to cause a storm of outgoing email from our cluster!!! This will lead to our server being blacklisted by Hopkins and/or other mail administrators.
    
    By default email notifications are sent for entire job arrays, not individual tasks. Be VERY careful if you change that behavior.

The mail arguments are shown in the [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html#OPT_mail-type) manual page.
    
```Shell title="Specify your email address" linenums="0"
--mail-user=<email-address>
```

```Shell title="Specify notification events" linenums="0"
--mail-type=<list-of-types>
```
Here are the main types:

- NONE, BEGIN, END, FAIL, INVALID_DEPEND
- ALL (equivalent to BEGIN, END, FAIL, INVALID_DEPEND, REQUEUE, and STAGE_OUT)
- TIME_LIMIT, TIME_LIMIT_90 (reached 90 percent of time limit), TIME_LIMIT_80 (reached 80 percent of time limit), TIME_LIMIT_50 (reached 50 percent of time limit)

## Output and error files
By default your job will store an output file named "slurm-%j.out" where the "%j" is replaced by the job ID containing job output and errors in the same directory in which your job ran. You can direct SLURM to put the file(s) elsewhere and change their names.

For job arrays, the default file name is "slurm-%A_%a.out", "%A" is replaced by the job ID and "%a" with the array index. 

If you add echo statements in your job batch file then you can inspect the job output/error files for clues as to its status.

## scontrol show job
The `scontrol` command has many uses. Before a job ends you can get detailed information with `scontrol`.

!!! YAY
    We have written a document describing frequent uses of scontrol. See [this page](tips-scontrol.md).
    
## squeue
Display information about pending and running jobs.

https://slurm.schedmd.com/archive/slurm-22.05.9/squeue.html

## sstat 
Display process statistics of a running job/step. Some of the advice given for `sacct` (below) applies to `sstat`.

https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html

## sacct
Display accounting data for running and completed jobs in the Slurm database.

!!! YAY
    We have written a document describing key elements of using sacct. See [this page](tips-sacct.md).

## Suggestions

The following was sent to a user who was having SAS jobs fail with errors indicating a problem with space or quota.

Instrumenting your job means adding commands to it to gather information. Running df commands to check the size and capacity of all or specific file systems. (df -h -x tmpfs might be a helpful variant). As mentioned in the orientation material, the sstat command can gather information for running jobs (as opposed to sacct which is oriented towards completed jobs). Some of the output fields available via sstat and sacct relate to memory usage while others can reveal disk input/output volumes.
 
Because your batch job script might not be able to run sstat’s during SAS’s execution, you may need to run the information-gathering commands interactively or via a second batch job. (I mean your batch job could run commands before or after invoking SAS, unless they put the SAS program into the background they won’t be able to run the commands while SAS is running. I don’t know what happens if one tries to run a program in the background (by putting an & after it, like one can run thunar for example) inside of a SLURM batch job.
 
Your gathering might involve the sleep command in between invocations. I would suggest also running the date command so you have timestamps.
 
You could put the sleep, date, sstat commands inside of a while (true) loop. Then cancel the loop with a control c or scancel depending on whether they are running interactive or batch. Redirecting the output to text files would be useful.
 
So your information-gathering efforts should probably start by running some sacct commands to inspect the parameters of previous failed jobs, and perhaps even the ones that succeeded to see if any differences appear.
 
The -e flag is available to both sstat and sacct to show the fields available. The sets overlap but also differ.
