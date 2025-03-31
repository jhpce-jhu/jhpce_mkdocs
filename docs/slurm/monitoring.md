---
tags:
  - in-progress
  - slurm
---

# **Monitoring SLURM Jobs**

## **Overview**
Has my job ended? Why did my job fail? How much memory did my sample job use so I can use memory efficiently in future similar jobs?[^1] There are a number of ways in which you can learn the answers to these questions. The state of the job will determine which tools and techniques you use.

[^1]: Making sure your jobs request the right amount of RAM and the right number of CPUs helps you and others using the clusters use these resources more efficiently, and in turn get more work done more quickly.

## **JOB STATES**

Until we draw a nice diagram, a written description will have to suffice. Here we focus on batch jobs and common states. Job states have short names consisting of one or two capitalized letters, and a full name. Short names are introduced in parentheses below. We have placed a copy of the list in `/jhpce/shared/jhpce/docs/job-states.txt`.

Batch jobs consist of several job steps, two at minimum ("batch"[^2] and "extern"[^3]). The overall job and each step has its own job state code. They often differ. The `-X` flag to sacct will show you only the overall job state, such as FAILED, but some times you need to check the state of all of a jobs steps in order to see that a "batch" step ran OUT_OF_MEMORY.

[^2]: The batch script that you have created to run your commands.
[^3]: The external step is the connection SLURM makes to the compute node to begin executing your job.

1. User submits job
2. SLURM evaluates syntax and resource requests.
3. If problems found, then job is rejected immediately.
4. Otherwise it is accepted and becomes PENDING (PD).
5. SLURM's scheduler algorithms begin testing where your job will "fit" in its planning process for the future. There will be more slots where smaller jobs (in RAM, CPU and duration) will fit than larger ones.
5. PENDING jobs can be held, CANCELLED (CA) or be dispatched to compute node(s) to start RUNNING (R).
6. RUNNING jobs can immediately run into a problem due to coding errors and become FAILED (F).
7. RUNNING jobs can run correctly but then exceed their memory allocation and become OUT_OF_MEMORY (OOM).
8. RUNNING jobs can run correctly but run into their wall-clock time limit and become DEADLINE (DL) or FAILED.
9. RUNNING jobs can run correctly, switch to COMPLETING (CG) as processes are quitting, and then COMPLETED (CD).


## **PENDING JOBS**

!!! Note "Authoring Note"
    Need to insert here the most common reasons and their explanations.

Jobs waiting to run will sit "in the queue." They will be shown to have a "Reason"

### **Tools for pending jobs**
`squeue --me`

`squeue --me --start`

`scontrol show job` - this only works for pending and running jobs.

## **RUNNING JOBS**

### **How to get information about running jobs?**
If you want or need to get more information about your jobs, you should add "instrumentation" to them. Instrumentation is any of a number of techniques which gather and save or print information for you to inspect. A trivial example is adding a command in your batch file script that echoes "I'm running on node" and then runs the hostname command.

1. `scontrol show job`
2. Look at their output and error files
2. Look at files they are writing to
3. Use `sstat` to inspect parameters SLURM has collected for the job
2. Use `sattach` to connect to the input/output of your script on the leading node of a job. (For advanced users. Using the `tail -f` command on job output files as the job runs is probably much more useful.)
3. Log into a node and inspect system state with commands like `ps` or `htop`


## **COMPLETED JOBS**
Here we mean jobs that are no longer running, whether they succeeded or failed for some reason.

### **How to get information about completed jobs?**
If you want or need to get more information about your jobs, you should add "instrumentation" to them. Instrumentation is any of a number of techniques which gather and save or print information for you to inspect. A trivial example is adding a command in your batch file script that echoes "I'm running on node" and then runs the hostname command.

1. Look at their output and error files
2. Look at files they wrote to
3. The command `seff jobid` for a given jobid will report on RAM and CPU efficiency. 
4. The `reportseff` command provides a way to inspect multiple jobs that match some criteria. See [our page](../slurm/tips-reportseff.md) about it.
4. Use `sacct` to inspect parameters about the job, including their "exitcode", memory usage, nodes used, etc. See [our page](../slurm/tips-sacct.md) about it.

Exit codes are poorly documented, unfortunately. See [this section](../slurm/tips-sacct.md#exit-error-codes) of the sacct tips page for what is known.

`scontrol show job` doesn't work for completed jobs.

## **INVESTIGATING FAILURES**

!!! Note "Authoring Note"
    This might warrant a dedicated page. THERE IS IMPORTANT INFO TO ADD -- regarding techniques, approaches. 

### **Exit codes**
In addition to the STATE of jobs and job steps, you can inspect the EXITCODE and a probably rarely useful DERIVEDEXITCODE fields with the `sacct` command.

The exit code of a job is captured by Slurm and saved as part of the job record. For sbatch jobs the exit code of the batch script is captured. For srun, the exit code will be the return value of the executed command. Any non-zero exit code is considered a job failure, and results in job state of FAILED. When a signal was responsible for a job/step termination, the signal number will also be captured, and displayed after the exit code (separated by a colon).

Depending on the execution order of the commands in the batch script, it is possible that a specific command fails but the batch script will return zero indicating success. **ONLY THE STATUS OF THE LAST COMMAND EXECUTED IS USED TO DETERMINE JOB SUCCESS OR FAILURE.**

!!! Caution "The echo command will produce a misleading job status for your-main-command"
    ```
    #!/bin/bash
    ...
    ...
    your-main-command < inputfile > outputfile
    echo "job finished"
    ```

## **INVESTIGATING RESOURCE CONSUMPTION**

!!! Note "Authoring Note"
    This might warrant a dedicated page. THERE IS IMPORTANT INFO TO ADD -- regarding techniques, approaches. 

People are primarily interested in reducing the amount of memory required to run a job, as memory contention is normally high in the cluster.

Until this section is further developed, see the sections below on the [seff](#seff-slurm-efficiency), [sstat](#sstat), and [sacct](#sacct) commands.

## **TOOL SET**

Here is a list of tools and techniques. You may use the same command to look at jobs in different states so we've gathered details here rather than repeating them in every section above.

### **Output and error files**
By default your job will create, in the same working directory where your job was submitted, an output file named "slurm-%j.out" (where the "%j" is replaced by the job ID) containing _both_ job output and error messages.

You can inspect this file during job execution. The `tail` command has a useful option `-f` which will print new lines as they appear.

You can direct SLURM to seperate normal output from error messages by specifying   the directives `-o` or `--output` and `-e` or `--error`. These directives also allow you to specify the desired location of these file(s) elsewhere and change their names.

For job arrays, the default file name is "slurm-%A_%a.out", "%A" is replaced by the job ID and "%a" with the array index. 

If you add `echo` statements in your job batch file at different points, then you can inspect the job output/error files for clues as to its status/progress.

### **sview, a GUI Interface**
Sview is an X11 program which will display information about the cluster. You can see information about jobs, nodes, partitions, and reservations.  Double-clicking on an object usually launches a pop-up window with more details.

It is not the slickest interface, and has quirks. It does not remember all of your choices for the basic items to display, for example (click on the tab labeled "visible tabs" to check the ones you want to see). Hopefully some of these issues will be addressed when we upgrade to a newer version of SLURM.

Explore the options available in various menus. Double-click on various objects to see what happens.

The `sview` manual page is available [here](https://slurm.schedmd.com/archive/slurm-22.05.9/sview.html

!!! Warning
    Do **NOT** set its refresh interval to 1 second. It defaults to 5 seconds, which is already quite bad enough. Sview contacts the master SLURM daemon running the entire cluster every *interval* seconds and requests a _ton_ of information. If enough people run it, it will slow down processing of everyone's jobs and accounting records and ... Do you really need to know about changes every five seconds? How will that change what you do?

### **Email Notification**

You can direct SLURM to send you email when various things happen with your job. These directives can be given on the command line, in batch job scripts, and set in your SLURM defaults file. You can even modify running jobs to set or change their notification settings (see the [scontrol tips](tips-scontrol.md) page).

!!! Warning
    Take care not to cause a storm of outgoing email from our cluster!!! This will lead to our server being blacklisted by Hopkins and/or other mail administrators. Then NO ONE will get email until we can convince them that it won't happen again.
    
    By default email notifications are sent for entire job arrays, not individual tasks. Be VERY careful if you change that behavior.

The mail arguments are shown in the [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html#OPT_mail-type) manual page.
    
```Shell title="Specify your email address" linenums="0"
--mail-user=<email-address>
```

```Shell title="Specify notification events" linenums="0"
--mail-type=<list-of-types>  # comma-separated
```

??? note "Here are the main types:"
    - NONE, BEGIN, END, FAIL, INVALID_DEPEND
    - ALL (equivalent to BEGIN, END, FAIL, INVALID_DEPEND, REQUEUE, and STAGE_OUT)
    - TIME_LIMIT, TIME_LIMIT_90 (reached 90 percent of time limit), TIME_LIMIT_80 (reached 80 percent of time limit), TIME_LIMIT_50 (reached 50 percent of time limit)

### **squeue**
[squeue](https://slurm.schedmd.com/archive/slurm-22.05.9/squeue.html) - Display information about pending and running jobs.

### **sstat** 
Display process statistics of a running job/step. Some of the advice given for `sacct` (below) applies to `sstat`.

The manual page is at [https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html](https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html)

### **scontrol show job**
The `scontrol` command has many uses. Before a job ends you can get detailed information with `scontrol`.

!!! YAY
    We have written a document describing frequent uses of scontrol. See [this page](tips-scontrol.md).
 
### **sacct**
    
`sacct` - Display accounting data for running and completed jobs in the Slurm database.

!!! YAY
    We have written a document describing key elements of using sacct. See [this page](tips-sacct.md).

### `seff` (Slurm EFFiciency)

!!! Warning
    seff output might not be correct for some jobs. If you need to double-check its output, gather the raw info with sacct.
    
CPU efficiency indicates how frequently allocated CPUs were busy. The more CPU-bound a job is, the closer to 100% this will be. The more input/output-bound a job is, the closer this will be to 0.

Memory efficiency indicates the proportion of requested memory used at the high-water mark. Values over 100% are not uncommon. It seems that such cases represent cases where the way in which shared memory (such as dynamically-linked code libraries) are counted, perhaps against multiple threads running on one CPU.

```
Job ID: 3946128
Array Job ID: 3560537_102
Cluster: jhpce3
User/Group: jmuschel/biostats
State: COMPLETED (exit code 0)
Cores: 1
CPU Utilized: 2-12:47:05
CPU Efficiency: 99.94% of 2-12:49:12 core-walltime
Job Wall-clock time: 2-12:49:12
Memory Utilized: 15.34 GB
Memory Efficiency: 76.71% of 20.00 GB
```

### `reportseff`

The `reportseff` command provides a way to inspect multiple jobs that match some criteria. See [our page](../slurm/tips-reportseff.md) about it.

## **Suggestions to a user
**
!!! Note
    The following was sent to a user who was having SAS jobs fail with errors indicating a problem with space or quota.

Instrumenting your job means adding commands to it to gather information. Running df commands to check the size and capacity of all or specific file systems. (df -h -x tmpfs might be a helpful variant). As mentioned in the orientation material, the sstat command can gather information for running jobs (as opposed to sacct which is oriented towards completed jobs). Some of the output fields available via sstat and sacct relate to memory usage while others can reveal disk input/output volumes.
 
Because your batch job script might not be able to run sstat’s during SAS’s execution, you may need to run the information-gathering commands interactively or via a second batch job. (I mean your batch job could run commands before or after invoking SAS, unless they put the SAS program into the background they won’t be able to run the commands while SAS is running. I don’t know what happens if one tries to run a program in the background (by putting an & after it, like one can run thunar for example) inside of a SLURM batch job.
 
Your gathering might involve the sleep command in between invocations. I would suggest also running the date command so you have timestamps.
 
You could put the sleep, date, sstat commands inside of a while (true) loop. Then cancel the loop with a control c or scancel depending on whether they are running interactive or batch. Redirecting the output to text files would be useful.
 
So your information-gathering efforts should probably start by running some sacct commands to inspect the parameters of previous failed jobs, and perhaps even the ones that succeeded to see if any differences appear.
 
The -e flag is available to both sstat and sacct to show the fields available. The sets overlap but also differ.
