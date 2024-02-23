---
tags:
  - slurm
---
# scontrol useful command examples
    
[Scontrol](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html) is a command useful to both users and systems administrators.  It is used to display and modify SLURM configuration. It has a number of sub-commands, which take different arguments. See the [index](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#index) for a pointer to the area of interest.

All  commands and options are case-insensitive, although node names, partition names, and reservation names are case-sensitive. All  commands  and options can be abbreviated to the extent that the specification is unique. 

Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.

## Scontrol for Users

### Show things
```Shell title="" linenums="0"
scontrol show --details job <jobid>
```

```Shell title="" linenums="0"
scontrol show node <nodename>
```

```Shell title="" linenums="0"
scontrol show partition <partitionname>
```

### [Update jobs](https://slurm.schedmd.com/scontrol.html#lbAH)

You can update many aspects of pending jobs, fewer for running jobs. What follows is only a sample!!!

#### Pending Jobs

```Shell title="Set max # of tasks in an array that execute at same time" linenums="0"
scontrol update jobid=<jobid> ArrayTaskThrottle=<count>
```
Users can change the time limit on their pending jobs. After a job starts to run, only a system administrator can adjust the time.

```Shell title="Set max job duration linenums="0"
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

## Scontrol for Systems Administrators

```Shell title="Modify debug level" linenums="0"
scontrol setdebug info # or verbose
```

```Shell title="Display running configuration" linenums="0"
scontrol show config
```

```Shell title="" linenums="0"
scontrol update partitionname=interactive allowqos=normal,interactive-default
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

