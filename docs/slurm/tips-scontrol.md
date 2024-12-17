---
tags:
  - slurm
---
# **scontrol -- useful command examples**
    
[Scontrol](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html) is a command useful to both users and systems administrators.  It is used to display and modify SLURM configuration. It has a number of sub-commands, which take different arguments. See the [index](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#index) section of the manual page for a pointer to the area of interest (what kind of thing you want to see or modify).

All  commands and options are case-insensitive, although node names, partition names, and reservation names are case-sensitive. All  commands  and options can be abbreviated to the extent that the specification is unique. 

Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.

## **Scontrol for Users**

### **Show things**
```Shell title="" linenums="0"
scontrol show --details job <jobid>
```

```Shell title="" linenums="0"
scontrol show node <nodename>
```

```Shell title="" linenums="0"
scontrol show partition <partitionname>
```

### **Update Jobs**

You can update many aspects of pending jobs, (more items than for running jobs). What follows is only a sample!!! Click [here](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#lbAH) for the complete list.

It can be to your benefit to update existing pending jobs rather than `scancel` them and re-submit them. For example, jobs accrue priority points as they sit waiting in the queue.  Those points are lost if you cancel the job.

You can find the commands necessary to add a Quality of Service attribute to a pending job in our [QOS document](../slurm/qos.md).

#### **Pending Jobs**

```Shell title="Add a reservation to one of your jobs" linenums="0"
scontrol update jobid=<jobid> reservation=<reservation-name>
```
```Shell title="Remoive a reservation from one of your jobs" linenums="0"
scontrol update jobid=<jobid> reservation=
```

```Shell title="Place one of your jobs ahead of other of your jobs" linenums="0"
scontrol top <jobid>
```

```Shell title="Place one of your jobs ahead or behind other of your jobs" linenums="0"
scontrol update jobid=<jobid> nice=<adjustment> # larger #s decrease the priority
```

```Shell title="Set or modify max # of tasks in an array that execute at same time" linenums="0"
scontrol update jobid=<jobid> ArrayTaskThrottle=<count>
```
Users can change the time limit on their pending jobs. After a job starts to run, only a system administrator can adjust the time.

```Shell title="Set max job duration" linenums="0"
scontrol update jobid=<jobid> TimeLimit=<time-specification>
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

#### **Running Jobs**
These can also be used on pending jobs. They're just examples of something you might want to set afterwards.

```Shell title="Be notified at 80% of job duration" linenums="0"
scontrol update jobid=<jobid> mailtype=time_limit_80
```
```Shell title="But only if you tell it where to send email" linenums="0"
scontrol update jobid=<jobid> mailuser=<your-address@jh.edu>
```

## **Scontrol for Systems Administrators**

### **Resuming a node**
Perhaps the most common use is to put a node back into operation that was marked DRAIN by `slurmctld` because it noticed a problem with a job. There is a `resume` shell script that accepts a three digit node number.

```Shell title="Put a DOWN/DRAIN node back into service" linenums="0"
scontrol update nodename=compute-112 state=resume reason="JHPCE: Fixed sssd problem"
```

### **Modifying jobs or partitions**

It can be useful to modify a user's pending job. You can give a pending job a different priority or add a QOS as well as more normal things like changing the endtime of a running job. The manual page [section on updating jobs](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#lbAH)

### **On-the-fly modifications to SLURM**

One can modify a number of overall configuration parameters normally defined in slurm.conf. Node and partition definitions can be changed, but not the amount of RAM or CPUs on a node!!!

However, this change is temporary, and will be overwritten whenever the daemon (`slurmctld` on the server or `slurmd` on a client) reads the slurm.conf configuration file. Since multiple system administrators are involved, someone might push out with Ansible a revised configuration file which will restart all of the server and client demons. 

```Shell title="Display running configuration" linenums="0"
scontrol show config
scontrol show config | grep -i debug # show debug, debugflags
```

```Shell title="Modify debug level" linenums="0"
scontrol setdebug info [nodes=<nodelist>]
# (info is our normal level, so please return to that after debugging)
# values in order: quiet,fatal,error,info,verbose,debug,debug2 thru debug5
```
If you specify any nodes, then that node's slurmd will be modified rather than the master slurmctld. It is unclear which flags will produce meaningful information on compute nodes since most often we are interested in the decisions made by the scheduler, which is in slurmctld. But the debug level is probably useful sometimes.

```Shell title="Modify debug flags" linenums="0"
scontrol setdebugflags + | - FLAG [nodes=<nodelist>]
# The plus or minus is required
# Possibly most interesting flags:
# Backfill, BackfillMap, CPU_Bind, Gres, NodeFeatures, Priority, Reservation, Steps, TraceJobs
```
There is a warning that some debugflags require restarting slurmctld, which means using them requires editing slurm.conf (because scontrol's changes are to the run-time environment...). It would be nice if they told us which ones.


```Shell title="Modify a partition" linenums="0"
scontrol update partitionname=interactive allowqos=normal,interactive-default
```

### **Reservations**

Reservations can be made for a number of purposes and in a number of ways. See the `scontrol` manual page for more details - [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#lbAQ) is about reservations. There is also the [Advanced Resource Reservation Guide](https://slurm.schedmd.com/archive/slurm-22.05.9/reservations.html)

Can be used to prevent a node from accepting jobs after reboot. (Our nodes have root cronjobs that try to set their state to RESUME at reboot.)

Can be used to create repeating reservations, which start and expire at certain times. This can be used to ensure that resources are avilable for class sessions. Note though that the scheduler will prevent jobs from starting if their job duration means they will run over into these windows. (The start time will be reset by the scheduler to be right after the reservation will end (unless the job's duration is such that it will end in another instance of this repeating reservation!!!  This might create situations that are hard for both users and sysadmins to understand.)

Another use: A node has file-system mounting issues that cause new jobs which try to use a missing file system to die. But the node has running jobs that you want to allow to finish.  You can set the node to DRAIN. You can also create a reservation, which prevents the node from accepting new jobs, and after attempts to fix it, allows you to verify that things are back to normal.  The reservation name can be self-documenting as to why the node is in the condition it is. SLURM will set nodes into DRAIN on its own for some types of problems it can detect, but it doesn't create reservations automatically.

When creating a reservation, you MUST select:

1. a **starttime** -- Reservations have start times. You can create reservations that, for example, take nodes out of service a month from now for a planned maintenance window. (The scheduler will prevent jobs from starting if they would run over into the reserved time.)
2. a **duration** -- reservation need to expire at some point. (Note that "UNLIMITED" turns out to be one year.)
3. a **list of users** OR a **group name** -- You cannot mix users and groups. If a research group name exists it can be easier than listing many users.
4. the **nodes** or **partitions** being reserved

When creating a reservation, you CAN select:

1. a meaningful **name** -- `scontrol` will generate a name mechanically if you don't specify one. That's not very informative. Users will have to specify the name when submitting jobs. Sysadmins have to specify the name to modify or end it. Names cannot contain spaces. Use hyphens to make it more readable.
2. Resources -- Versions of SLURM newer than 22.05.09 may allow the reservation of CPU, RAM and/or GPU resources (via TRES arguments). If so, you could carve out "space" for a user to run certain jobs on any of the nodes in a partition without locking *everyone* out of the partition. That would be handy.


```Shell title="Show existing reservations" linenums="0"
scontrol show reservation
```
Here is a reservation that prevents normal users from using a node for a year, starting now. System administrators are able to use the reservation to test the node.

```Shell title="Create a reservation" linenums="0"
scontrol create reservation starttime=now duration=UNLIMITED user=root,tunison,mmill116,jyang flags=maint,flex,ignore_jobs,NO_HOLD_JOBS_AFTER reservation=resv-name nodes=compute-number
```

```Shell title="Add a user to an existing reservation" linenums="0"
scontrol update reservation=<resv-name> user+=<username>
```

```Shell title="Create downtime reservations on entire partitions" linenums="0"
grep -v "^#" /etc/slurm/partitions.conf|grep PartitionName|grep -v DEFAULT|awk '{print $1}'|cut -c 15- > /tmp/mylist

for part in `cat /tmp/mylist`;
do
scontrol create reservation=power-outage-$part flags=maint,ignore_jobs,part_nodes starttime=2024-07-15T17:00 endtime=2024-07-22T17:00 nodes=all users=root partitionname=${part}
done
```

#### **Ending Reservations**

```Shell title="Delete a reservation" linenums="0"
scontrol delete reservation=<resv-name>
```

If you are prevented from deleting a reservation because jobs are running, you can modify the endtime. Note that it will take a few seconds or a minute to see the reservation go away.

```Shell title="Another way to delete a reservation" linenums="0"
scontrol update reservation=<resv-name> endtime=now
```

#### **Flags**

Some flags are important to include. Capitalized here to make them stand out.

* **MAINT** - this is optional. The node's state will include MAINT in addition to other possible states (IDLE, DRAIN, DOWN). The downtime won't be counted by SLURM in stats about cluster uptime. But perhaps more importantly for us: "This reservation is permitted to use resources that are already in another reservation." Although there is also **OVERLAP**
* **IGNORE_JOBS** - Ignore currently running jobs when creating the reservation. Otherwise you will be prevented from creating the reservation.
* **NO_HOLD_JOBS_AFTER** - without it pending jobs requesting a reservation will be changed to "held" if a reservation ends. Held jobs require manual intervention. There are user and system administration holds -- don't know which is used in the case of not using this flag.
* **MAGNETIC** - Allows jobs to be considered for this reservation even if they didn't request it. (Would this allow jobs sent to a partition which has a reservation on it to run if the user forgot or didn't know to include a reservation directive?) 
* **FLEX** - Permit jobs requesting the reservation to begin prior to the reservation's start time, end after the reservation's end time, and use any resources inside and/or outside of the reservation regardless of any constraints possibly set in the reservation. A typical use case is to prevent jobs not explicitly requesting the reservation from using those reserved resources rather than forcing jobs requesting the reservation to use those resources in the time frame reserved. Another use case could be to always have a particular number of nodes with a specific feature reserved for a specific account so users in this account may use this nodes plus possibly other nodes without this feature.
