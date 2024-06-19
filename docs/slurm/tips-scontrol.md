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

You can update many aspects of pending jobs, fewer for running jobs. What follows is only a sample!!! Click [here](https://slurm.schedmd.com/scontrol.html#lbAH) for the complete list.

You can find the commands necessary to add a QOS to your job specification or to update a pending job in our [QOS document](../slurm/qos.md).

#### **Pending Jobs**

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

Another use: A node has file-system mounting issues that cause jobs which try to use a missing file system to die. But the node has running jobs that you want to allow to finish. A reservation prevents the node from accepting new jobs. So would setting the node to DRAIN. The reservation name can be self-documenting as to why the node is in the condition it is. SLURM will set nodes into DRAIN on its own but it doesn't create reservations automatically.

When creating a reservation, you should select:

1. a duration -- reservations can expire. Note that "UNLIMITED" turns out to be one year.
2. a starttime -- you can create reservations that begin into the future to, for example, take nodes out of service for maintenance. (I think that the scheduler will prevent jobs from starting if they would run over into the reserved time.)
2. a list of users or a group name, if available. You cannot mix users and groups. If a research group name exists it can be easier than listing many users.
3. a meaningful name -- users will have to specify this name when submitting jobs. Sysadmins have to specify the name to modify or end it.
4. nodes or partitions being reserved
5. Resources -- Newer versions of SLURM than 22.05.09 may allow the reservation of _both_ CPU and RAM resources (via TRES arguments). If so you could carve out "space" for a user to run certain jobs on any of the nodes in a partition without locking everyone out of the partition. That would be handy.


```Shell title="Show any reservations" linenums="0"
scontrol show reservation
```
Here is a reservation that prevents normal users from using a node for a year, starting now. System administrators are able to use the reservation to test the node.

```Shell title="Create a reservation" linenums="0"
scontrol create reservation starttime=now duration=UNLIMITED user=root,tunison,mmill116,jyang flags=maint,flex,ignore_jobs,NO_HOLD_JOBS_AFTER reservation=resv-name nodes=compute-number
```

```Shell title="Add a user to an existing reservation" linenums="0"
scontrol update reservation=<resv-name> user+=<username>
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
* **FLEX** - Permit jobs requesting the reservation to begin prior to the reservation's start time, end after the reservation's end time, and use any resources inside and/or outside of the reservation regardless of any constraints possibly set in the reservation. A typical use case is to prevent jobs not explicitly requesting the reservation from using those reserved resources rather than forcing jobs requesting the reservation to use those resources in the time frame reserved. Another use case could be to always have a particular number of nodes with a specific feature reserved for a specific account so users in this account may use this nodes plus possibly other nodes without this feature.
