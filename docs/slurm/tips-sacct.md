---
tags:
  - in-progress
  - slurm
---
# sacct useful command examples
    
[sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html) is a command used to display information about jobs. It has a number of subtleties, such as the time window reported on and the formatting of output. We hope that this page will help you get the information you need.

Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.

!!! Warning
    This page is under HEAVY construction. It was created by copying the scontrol tips page.
    
## sacct basics

1. By default only your own jobs are displayed
2. Only jobs from a certain time window are displayed by default. The window varies depending the arguments you provide. See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAH) of the manual page.
3. You can control the width of output fields 
4. Even the simplest of batch jobs contain multiple "steps" as far as SLURM is concerned. One of them, named "extern" represents the ssh to the compute node on behalf of your job. Job records consist of a primary entry for the job as a whole  as  well as entries for job steps. The [Job Launch](https://slurm.schedmd.com/archive/slurm-22.05.9/job_launch.html#job_record) page has a more detailed description of each type of job step.

!!! Warning
    Sacct retrieves data from a SQL database. Be careful when creating your sacct commands to limit the queries to the information you need. That database needs to be modified constantly as jobs start and complete, so we don't want it tied up answering sacct queries. If you want to look at a large amount of data in a variety of ways, consider saving the output to a text file and then working with that file.


### Available fields

Fields are explained in [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAF) of the manual page.

```Shell title="" linenums="0"
sacct -e
```

```Shell title="Show some basic fields" linenums="0"
sacct --allusers -o "user,jobid,state,nodelist,submit,start,end,exitcode,state"
```

```Shell title="See all fields for job" linenums="0"
sacct -o ALL -j <jobid>
```

### Formatting fields

You can put a %NUMBER after a field name to specify how many characters should be printed, e.g.
   
- format=name%30 will print 30 characters of field name right justified.  
- format=name%-30 will print 30 characters left justified.

### Using Environment Variables

You can define environment variables in your shell to reduce the complexity of issuing sacct commands. You can also set these in shell scripts. Command line options will always override these settings.

SACCT_FORMAT

SLURM_TIME_FORMAT

#### Formatting Dates/Times
You can use most variables defined by the STRFTIME(3) system call. [This web page](https://strftime.org) is a starting point, but what SLURM has chosen to implement may not match.

* %a - abbrieviated name of day of the week
* %m - month as decimal, 01 to 12
* %d - day of month as decimal
* %H - hour as decimal in 24-hour notation
* %M - minute as decimal, 00 to 59
* %T - time in 24-hour notation (%H:%M:%S)

```Shell title="" linenums="0"
export SLURM_TIME_FORMAT="%a %m-%d %H:%M"   #  
```

```Shell title="Place one of your jobs ahead or behind other of your jobs" linenums="0"
scontrol update jobid=<jobid> nice=<adjustment> # larger #s decrease the priority
```

```Shell title="Set or modify max # of tasks in an array that execute at same time" linenums="0"
scontrol update jobid=<jobid> ArrayTaskThrottle=<count>
```
Users can change the time limit on their pending jobs. After a job starts to run, only a system administrator can adjust the time.

```Shell title="Set max job duration" linenums="0"
scontrol update jobid=<jobid> TimeMin=<time-specification>
```

```Shell title="Hold one of your jobs (to prefer other of your jobs)" linenums="0"
scontrol hold <job-list>  # Can be comma-separated list of jobids
```

```Shell title="Release a held job" linenums="0"
scontrol release <job-list>  # Can be comma-separated list of jobids
```

```Shell title="Lower the priority of one of your jobs (to prefer other of your jobs)" linenums="0"
scontrol update jobid=<jobid> nice=10
```

```Shell title="This is per-node, not per-job. In megabytes" linenums="0"
scontrol update jobid=<jobid> MinMemoryNode=1024
```

#### Running Jobs
These can also be used on pending jobs. They're just examples of something you might want to set afterwards.

```Shell title="Be notified at 80% of job duration" linenums="0"
scontrol update jobid=<jobid> mailtype=time_limit_80
```
```Shell title="But only if you tell it where to send email" linenums="0"
scontrol update jobid=<jobid> mailuser=<your-address@jh.edu>
```

## Flags of Interest

These flags are probably the ones you'll want. See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAF) of the manual page for the list and their meaning.

### Basics
- User
- State

### Resources Asked For, Consumed
- ReqTRES
- TRESUsageInTot
- ReqNodes
- ReqCPUS

### Times
- Submit
- Start
- Elapsed
- End

### Nodes
- AllocNodes
- NodeList



```Shell title="Modify debug level" linenums="0"
scontrol setdebug info # or verbose
```

```Shell title="Display running configuration" linenums="0"
scontrol show config
```

```Shell title="Modify a partition" linenums="0"
scontrol update partitionname=interactive allowqos=normal,interactive-default
```

```Shell title="Put a DOWN/DRAIN node back into service" linenums="0"
scontrol update nodename=compute-112 state=resume reason="Fixed sssd problem"
```

```Shell title="Show any reservations" linenums="0"
scontrol show reservation
```

```Shell title="Create a reservation" linenums="0"
scontrol create reservation starttime=now duration=UNLIMITED user=root,tunison,mmill116,jyang flags=maint,ignore_jobs,NO_HOLD_JOBS_AFTER reservation=resv-name nodes=compute-number
```

```Shell title="Delete a reservation" linenums="0"
scontrol delete reservation=<resv-name>
```

```Shell title="Another way to delete a reservation" linenums="0"
scontrol update reservation=<resv-name> endtime=now
```

```Shell title="Add a user to an existing reservation" linenums="0"
scontrol update reservation=<resv-name> user+=<username>
```

