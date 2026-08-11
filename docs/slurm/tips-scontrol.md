---
tags:
  - slurm
---
# scontrol -- Use and command examples
    
[Scontrol](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html) is a command useful to both users and systems administrators to display and modify SLURM configuration. It can work with several types of SLURM objects, and has a number of sub-commands for each type, each of which take different arguments.

* JOBS - showing, updating, cancelling (we have a `showjob` script - try it!)
* NODES - showing, creating, deleting, updating
* PARTITIONS - showing, creating, deleting, updating
* RESERVATIONS - showing, creating, deleting, updating

All  commands and options are case-insensitive, although node names, partition names, and reservation names are case-sensitive. All  commands  and options can be abbreviated to the extent that the specification is unique. 

!!! tip "Use the web manual page"
    The installed `scontrol` manual page (use `man scontrol`) can be hard to read. We suggest using the web version, which has an [Index](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#index) section at the bottom. The links there are very handy for getting to the right section, and the visual layout is much better. 

Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.

## **Scontrol for Users**

### **Get information**
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

You can update certain job parameters after submission. The job's STATE impacts which ones you can modify. You can modify more parameters if a job is PENDING than if it is RUNNING.

For example, you can change the job duration of a PENDING job but not once it has launched (otherwise you could submit a one hour job and then once it's grabbed a node, extend the maxtime to weeks, bypassing the scheduler's need to plan).

==The systems administrators can modify the job duration.== Please send requests to bitsupport@lists.jh.edu, preferably not at the last minute.

What follows is only a sample of the parameters that you can update!!! Click [here](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#lbAH) for the complete list.

It can be to your benefit to update existing pending jobs rather than `scancel` them and re-submit them. For example, jobs accrue priority points as they sit waiting in the queue.  Those points are lost if you cancel the job.

You can find the commands necessary to add a Quality of Service attribute to a pending job in our [QOS document](../slurm/qos.md).

#### **Pending Jobs**

```Shell title="Set max job duration" linenums="0"
scontrol update jobid=<jobid> TimeLimit=<time-specification>
```

```Shell title="Hold one of your jobs (to prefer other of your jobs)" linenums="0"
scontrol hold <job-list>  # <job-list> can be comma-separated list of jobids
```

```Shell title="Release a held job" linenums="0"
scontrol release <job-list>  # Can be comma-separated list of jobids
```

Sometimes users ask administrators to create a "reservation" of compute nodes so they can meet pressing deadlines. Jobs cannot access those resources unless they request it. Here is how you would change PENDING jobs to be able to use your reservation:

```Shell title="Add a reservation to one of your jobs" linenums="0"
scontrol update jobid=<jobid> reservation=<reservation-name>
```
To remove the reservation, do not include anything after the equals sign:

```Shell title="Remove a reservation from one of your jobs" linenums="0"
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


`

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
Perhaps the most common use is to put a node back into operation that was marked DRAIN by `slurmctld` because it noticed a problem with a job. There is a `resume` shell script that accepts either the nodename or its three digit node number.

```Shell title="Put a DOWN/DRAIN node back into service" linenums="0"
scontrol update nodename=compute-112 state=resume reason="JHPCE: Fixed sssd problem"
```

### **Modifying jobs**

See the earlier section for users about this. You can give a pending job a different priority or add a QOS as well as more normal things like changing the maxtime of a running job. See the manual page [section on updating jobs](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#lbAH)

```Shell title="Modify a partition" linenums="0"
scontrol update partitionname=interactive allowqos=normal,interactive-default
```

### **On-the-fly modifications to SLURM**

These can be **very useful** tools. Of course you need to be very careful.

You should keep in mind three types scenarios, because they impact where and what commands are run:

1. changing SLURM server settings concerning its debugging output or job scheduling,
2. changing the debug levels on compute nodes, and
3. changes to "objects" like nodes and partitions which require commands to be run on both the server and one or more compute nodes.

#### General caveats

Remember that both the SLURM server (running `slurmctld`) and the partition's member nodes (running `slurmd`) involved with what you're testing typically must have identical definitions of certain things. Some discrepancies are just noted in log files whereas other ones will either break everything or prevent your modification from working.

On-the-fly changes are temporary, and will be overwritten whenever the daemon (`slurmctld` on the server or `slurmd` on a client) reads the configuration files when restarted.  Since multiple system administrators are involved, someone might push out with Ansible a revised configuration file and restart all of the server and client demons. 

#### Modifying or setting SOME variables

One can modify a number of overall configuration parameters normally defined in `slurm.conf`. But not all, because some are core settings, and you wouldn't want to change them on a live cluster even if you did it by editing & pushing files and restarting daemons.

What are those things? Our `slurm.conf` files do not include every possible parameter. Also, you might need to compare the values on the server and a compute node to be **sure** of what each thinks.

```Shell title="Display running configuration of SLURM itself" linenums="0"
scontrol show config
scontrol show config | grep -i debug # will show debug and debugflags
``` 

Again, you can and/or should only change certain things. Existing variables with definitions in units should probably only be changed in an upwards manner -- you wouldn't want to cut the legs out from under pending or running jobs, such as lowering MaxArraySize from 10,000 to 400.

Examples of variables you might change KillOnBadExit, KillWait, MaxArraySize, SchedulerParameters, SchedulerTimeSlice.

You would most certainly not want to change settings like `SelectTypeParameters    = CR_CORE_MEMORY` even if you were allowed to.

JRT has successfully run a modification command on both the SLURM server and one test compute node, and tested it by adding `--nodelist=<that_node>` to his test jobs.

##### Nodes and partitions

**SOME** things about nodes can be changed, but not (1) the existence of a node or (2) the amount of RAM or CPUs on a node!!!

You can probably even create partitions on the fly, but more common things you might change would be PriorityTier or AllowGroups or AllowQos.

##### Debugging levels and flags

A **very useful** change is to increase and decrease the debug level of either or both the `slurmctld` daemon and the `slurmd` daemon on particular nodes. 

!!! danger "Use only as needed for slurmctld"
    YOU MUST QUICKLY REDUCE THE DEBUG LEVEL OF SLURMCTLD, BECAUSE IT CAN CONSUME ENORMOUS AMOUNTS OF LOG FILE SPACE.

```Shell title="Modify debug level" linenums="0"
scontrol setdebug info [nodes=<nodelist>]
# (info is our normal level, so please return to that after debugging)
# values in order: quiet,fatal,error,info,verbose,debug, and debug2 thru debug5
```
If you specify any nodes, then that node's `slurmd` will be modified rather than the master `slurmctld`.  You will have to test the impact of different levels to see whether the information you seek becomes visible.  Information about scheduler decisions is not as clear as you might hope, and of course it will only appear from `slurmctld`.

```Shell title="Modify debug flags" linenums="0"
scontrol setdebugflags + | - FLAG [nodes=<nodelist>]
# The plus or minus is required
# Possibly most interesting flags:
# Backfill, BackfillMap, CPU_Bind, Gres, NodeFeatures, Priority, Reservation, Steps, TraceJobs
```
There is a warning in the documentation that some `debugflags` require restarting `slurmctld`, which means using them requires editing `slurm.conf` and restarting as opposed to being able to do it on the fly with scontrol. It sure would be nice if they told us which ones!!

### **Reservations**

Please make reservation names self-documenting by including data like the problem a node is having. Reservation names must be unique, so adding the node number to the beginning allows you to tag multiple nodes with a common problem by creating reservations as the problems surface.

Reservations can be made for a number of purposes and in a number of ways. See the `scontrol` manual page for more details - [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html#lbAQ) is about reservations. There is also the [Advanced Resource Reservation Guide](https://slurm.schedmd.com/archive/slurm-22.05.9/reservations.html)

#### Reasons to create a reservation

* Create reservations to **prevent jobs from running during planned outages**.

See the script `/jhpce/shared/jhpce/core/JHPCE_tools/3.0/bin/timeleft` for details about how to do this. There is also a command example below to update all of the partitions at once.

SLURM will automatically schedule jobs to start after the outage if the job's end time will extend past the beginning of the outage.  The script was written to allow users to create jobs which will end before the outage so they can get as much work done as possible.

* **Users need specific amounts of resources** for certain amounts of time. Best practices: Ask them to specify only what they need, rather than, say, whole partitions. Ask them to tell you when they're finished, so you can release the resources back to the others. Consider asking them if there is an existing UNIX group who should be part of the reservation in case they ask for only themselves.

* **A node has problems**, like file-system mounting issues that cause new jobs which try to use a missing file system to die.

You want to prevent it from accepting jobs. You could set it node to DRAIN, but that isn't as clear of a signal to other administrators that something was intentionally done as opposed to SLURM deciding that the node has issues. Also, creating a reservation keeps jobs from landing on a node even if it reboots as part of your efforts. (Our nodes have root cronjobs that will set their state to RESUME at reboot.) 

* Create **repeating reservations**, which start and expire at certain times. This can be used to ensure that resources are available for class sessions. This might create situations that are hard for both users and sysadmins to understand. 

Note though that the scheduler will prevent jobs from starting if their job duration means they will run over into these windows. (The start time will be reset by the scheduler to be right after the reservation will end (unless the job's duration is such that it will end in another instance of this repeating reservation!!!  )

#### Requirements

When creating a reservation, you MUST select:

1. a **starttime** -- Reservations have start times. You can create reservations that, for example, take nodes out of service a month from now for a planned maintenance window. (The scheduler will prevent jobs from starting if they would run over into the reserved time.)
2. a **duration** -- reservation need to expire at some point. (Note that "UNLIMITED" turns out to be one year.)
3. a **list of users** OR a **group name** -- You cannot mix users and groups. If a research group name exists it can be easier than listing many users.
4. the **nodes** or **partitions** being reserved
5. certain flags (see below).

When creating a reservation, you CAN select:

1. a meaningful **name** -- `scontrol` will generate a name mechanically if you don't specify one. That's not very informative. Users will have to specify the name when submitting jobs. Sysadmins have to specify the name to modify or end it. Names cannot contain spaces. Use hyphens to make it more readable.
2. Resources -- Versions of SLURM newer than 22.05.09 may allow the reservation of CPU, RAM and/or GPU resources (via TRES arguments). If so, you could carve out "space" for a user to run certain jobs on any of the nodes in a partition without locking *everyone* out of the partition. That would be handy. ==The examples shown here do not include that future functionality.==


```Shell title="Show existing reservations" linenums="0"
scontrol show reservation
```
Here is a reservation that prevents normal users from using a node for a year, starting now. Some system administrators are able to use the reservation to test the node.

```Shell title="Create a reservation" linenums="0"
scontrol create reservation starttime=now duration=UNLIMITED \ user=root,tunison,mmill116,jyang \ flags=maint,flex,ignore_jobs,NO_HOLD_JOBS_AFTER \ reservation=<resv-name> nodes=compute-number
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

Some flags are important to include. They are capitalized here to make them stand out -- use whatever case you want in operation.

* **FLEX** - ==**Essential**== We want all of these benefits. FLEX permits jobs requesting the reservation to overlap boundaries of both time and the group of nodes in the reservation. Therefore jobs can
   * begin prior to the reservation's start time,
   * end after the reservation's end time, and
   * use any nodes inside and/or outside of the reservation.
   
   ==Without FLEX, jobs will be killed the moment a reservation ends==
   
   FLEX allows for smooth backfilling. 
   SLURM can schedule and start reservation-linked jobs early if the nodes happen to clear out ahead of schedule.

* **IGNORE_JOBS** - Ignore currently running jobs when creating the reservation. Otherwise you will be prevented from creating the reservation.

* **NO_HOLD_JOBS_AFTER** - Without it,  PENDING jobs requesting a reservation will be silently changed to "held" if a reservation ends. That's bad. Held jobs require manual intervention, which leads to confusion and delays. These are user holds, so either they or administrators can remove them. Also, if not addressed, they will sit in the queue forever, impacting SLURM's ability to save job state (the number of simultaneous jobs is capped by both a setting in slurm.conf AND the number which whose metadata will fit into a limited amount of RAM).

* **MAINT** - The reason to include it is that it allows this reservation to use resources that are already in another reservation. You could also use **OVERLAP** to do this. Using MAINT instead means that the node's STATE will include MAINT in addition to other possible states (IDLE, DRAIN, DOWN, RESERVED). 

Also, the nodes in MAINT are considered by the statistics used by `sreport` to be out of service, which impacts cluster uptime. We almost never run sreport.

* **MAGNETIC** - Allows jobs to be considered for this reservation even if they didn't request it. (Would this allow jobs sent to a partition which has a reservation on it to run if the user forgot or didn't know to include a reservation directive?)
