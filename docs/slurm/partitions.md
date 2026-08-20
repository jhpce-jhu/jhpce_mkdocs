---
tags:
  - slurm
---


# Partitions

A partition is a logical collections of nodes that comprise different hardware resources and limits to help meet the wide variety of jobs that get scheduled on the cluster. Nodes usually belong to more than one partition.

There are several *types* of partitions:

* **Public - general access** (e.g. shared, interactive, gpu, transfer)
* **Application only** (e.g. sas)
* **GPU** (equipped with GPU cards)
* **PI-owned** (for use only by members of the PI's group)

!!! Tip "slurmpic is a vital tool"
    Our command `slurmpic` shows information about partitions, including the member nodes, their current utilization, and some summary statistics. You should use it regularly. More details are given below.
    
## Choosing Partitions For Your Jobs
{==You should only submit jobs to partitions that you are entitled to use.==}

If you don't specify a partition:

- In the JHPCE cluster your job will go into the **shared** partition
- In the JADE cluster your job will go into the **N-shared** partition, where "N" is the community letter, such as "**c-shared**" for CMS community members. (The only other type of partition in JADE is ones for SAS)

!!! Warning "This page not yet rewritten to describe JADE"
    Details about specific partitions on this page are almost entirely about those found in the main JHPCE cluster. Until then, know that, in addition to the above about shared, the JADE partitions include: **c-sas** (application-only, for CMS users), **c-lau** (PI, for researchers in the Lau/Joshu/Rudolph group), **sql** (PI, for a database administrator for that same group), and **sysadmin** (for our testing). There are no GPU cards in JADE.

### Consider Submitting Non-batch Jobs To Multiple Partitions

**Jobs can be submitted to multiple partitions to increase the odds that they will start more quickly.** They will generally start in the first partition that has the required resources. There are a number of ways in which partitions are configured which may nudge a pending job to one partition over others. Pending jobs in some partitions are evaluated ahead of others. Priorities can be set on some partitions (such as `interactive`) as described [here](../slurm/whenstart.md/#partition-priority).

Simply separate partition names with commas (and no spaces!!). For example, in a batch job file:

```
#SBATCH --partition=cancergen,shared
```

This tactic is useful for several cases:

- If you're trying to get a small interactive job running on a node to quickly test something, you might include two or more of: **shared**, **interactive**, **interactive-larger** or **scavenge** partitions.

- If you are elible to use a PI partition, you might include your PI partition and shared.

!!! Warning "Batch jobs becp,e locked into one partition"
    All of the members of array jobs will be run into the first partition chosen for the first element. This sad fact means you cannot trivially spread child jobs around for maximum throughput. You might consider breaking your array down into chunks and submit each to one or multiple partitions.
    For example, if one had a 2,000 element array batch job, and they submitted it to shared and scavenge, and the first element ran on the scavenge partition, then the shared partition would never be used. The scavenge partition is described later, but here the thing to know is that it consists of only a few nodes.
    (Technical note for systems administrators: Slurm job arrays lock into a single partition because the scheduler modifies the master job record during the initiation of the first task, replacing the original comma-separated list of partitions with the specific partition that allowed the task to start first. Subsequent tasks in the array then inherit this single partition, preventing them from utilizing other, potentially idle partitions originally requested.)

## PI Partitions
JHPCE exists because Primary Investigators worked together to create a cluster. They share their resources via public partitions to support their fellow faculty members and to reduce their cost of ownership (see cost recovery descriptions [here](../aboutus/model.md/#cost-recovery) and [here](../joinus/new-pi.md)).

{==Only submit jobs to these partitions if you are a member of the Primary Investigator's research groups or have been given explicit permission to do so.==} If you are in doubt, ask before submitting. Jobs from non-group members will be killed and repeated abuse *will* lead to repercussions.  

## Public Partitions 

Partitions **shared**, **interactive**, **interactive-larger**, **gpu**, **sas**, **scavenge** and **transfer** are considered public and available to all.

!!! Note "Specific use"
    Only jobs which require the use of GPU cards should be submitted to the **gpu** partition.

    Only jobs which require the use of the SAS application should be submitted to the **sas** partition.
    
    Only jobs related to transferring data into or out of the cluster should be submitted to the **transfer** partition.

The public partitions provide low-priority access to unused capacity throughout the cluster. Capacity on the shared queue is provided on a strictly “as-available” basis and serves two purposes:

First it provides surge capacity to stakeholders who temporarily need more compute capacity than they own, and second, it gives provides non-stakeholders access to computing capacity.

Scheduling polices attempt to harvest unused capacity as efficiently as possible while mitigating the impact on the rights of stakeholders to use their resources. The JHPCE service center does not guarantee that stakeholders will provide sufficient excess capacity to meet non-stakeholders needs, however in practice the cluster is rarely operating at full capacity so there is usually ample computing capacity on the shared queue.

## Getting Info About Partitions

### What Partitions Exist?

You can view a list with this command:<br>

```bash linenums="0"
list-partitions
```

You will not see any partitions which have either been marked hidden (which we haven't done) or have a rule saying the partition can only be used by members of a UNIX users group. The latter is true for a few "PI partitions".

You can view the details with a specific one with this command:<br>

```
scontrol show partition <partitionname>
```
(For tips about using `scontrol`, see our [local scontrol tips](../slurm/tips-scontrol.md) page.)

### A Better View of Partitions

Our command `slurmpic` shows information about partitions, including the member nodes, their current utilization, and some summary statistics. Text is color-coded to try to indicate how fully consumed nodes are. ==You should run slurmpic regularly to get a feeling for the state of the cluster.==

Run `slurmpic -h` to see important usage notes!

**Which partitions to you see?**

- By default `slurmpic` displays one partition, while `slurmpic -a` shows all partitions. 
- In the JHPCE cluster: slurmpic defaults to the **shared** partition
- In the JADE cluster: slurmpic defaults to the **N-shared** partition, where "N" is the community letter, e.g. "**c-shared**" for CMS community members.
- Specific partitions can be displayed using: `slurmpic -p <partitionname>`
- All of the nodes in all of the GPU partitions can be displayed with `slurmpic -g`.
- Individual GPU-containing partitions can be shown with `slurmpic -g -p <partitionname>`
- You will not see hidden or restricted partitions you cannot submit to.

**About the displayed summary statistics:**

- They are for a single partition, not the whole cluster (except for `slurmpic -a`)
- They do not include the resources of hidden or restricted partitions.
- Memory and CPU use of nodes that are DOWN or in DRAIN are not included in the stats.

`slurmpic` uses the SLURM command [`sinfo`](https://slurm.schedmd.com/archive/slurm-22.05.9/sinfo.html), which shows information about nodes and partitions.

## CPU Partitions

### Public CPU Partitions

The CPU and RAM limits for `shared` change depending on node availability. We change the QOS `shared-default` up and down based on usage over the past month or two. We try to keep limits as high as possible to increase the utilization rate of the cluster while not allowing one person to dominate the `shared` partition. You can learn more about QOS [here](../slurm/qos.md) and see the currently defined QOS values with the command `showres`

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Core | RAM | Time | Notes/Use |
| ---- | :----: | ---- | ---- | :-------: | ----- |
| shared | public | 400 | 2.5TB | (1d/90d) | DEFAULT |
| interactive | public | 2 | 20gb | (1d/90d) | Small but accessible |
| interactive-larger | public | 8 | 80gb | (1d/5d) | Limit 2 jobs per user |
| gpu | public | (none) | (none) | (1d/90d) | Only for GPU jobs!!! |
| sas | application | (none) | (none) | (1d/90d) | Licensed for SAS |
| scavenge | public | 11 per node | 250G per node | (1d/5d) | See below |
| transfer | public | (none) | (none) | (none/90d) | Data in or out of cluster via SLURM jobs |

To reduce table width, column names are terse.

{==Experimental `interactive-larger` partition:==} Our interactive partitions give jobs a higher priority and are evaluated before those in other partitions. This "larger" one was created to try to allow people to run interactive jobs a bit larger in size than our smaller `interactive` partition. This is meant to help you explore data visually, debug a batch script that has failed by running one command at a time (on a small-enough data set), etc.

{==`scavenge` partition:==} This was created to try to "harvest" some typically-unused CPU and RAM resources from specific GPU nodes.  To protect the ability to run GPU jobs, only a fixed amount of each node can be used by _all of the jobs which land on it_. You can specify it along with your normal partition, e.g. `--partition=shared,scavenge` and it will be used if available. If your job's CPU, RAM, or duration parameters exceed those set for `scavenge` then your job will run on the other partition(s) you specified.

### PI CPU Partitions

To see the member nodes and resources of the any partitions, use `slurmpic -p partitionname`

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Core | RAM | Time | Notes/Use |
| ---- | :----: | ---- | ---- | :-------: | ----- |
| bader | PI | (none) | (none) | (none/90d) | |
| cancergen | PI | (none) | (none) | (none/90d) | |
| caracol | PI | (none) | (none) | (none/90d) | access enforced by UNIX group |
| cee | PI | (none) | (none) | (none/90d) | |
| cegs2 | PI | (none) | (none) | (none/90d) | |
| chatterjee | PI | (none) | (none) | (none/90d) | |
| echodac | PI | (none) | (none) | (none/90d) | |
| hongkai | PI | (none) | (none) | (none/90d) | |
| katun | PI | (none) | (none) | (none/90d) | access enforced by UNIX group |
| local-scharpf | PI | (none) | (none) | (1d/90d) | cancergen nodes with local SSD<BR>access enforced by UNIX group |
| momme | PI | (none) | (none) | (none/90d) | |
| stanley | PI | (none) | (none) | (none/90d) | access enforced by UNIX group |
| sysadmin | admin | (none) | (none) | (none/90d) | For system testing<BR>access enforced by UNIX group |


## GPU Partitions
To see the member nodes and resources of the any partitions, use `slurmpic -g`

To learn more about the GPU card types and how to use them, see [https://jhpce.jhu.edu/gpu/gpu-info/](https://jhpce.jhu.edu/gpu/gpu-info/)

The Biostatistics partitions are for anyone who is sponsored by a PI in that department.

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Requires Approval | Core | RAM | GPU | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- | ------|
| gee | PI | yes | (none) | (none) | (none) | (none/90d) | |
| gpu | public | no | (none) | (none) | (none) | (1d/90d) | the ONLY public GPU resource<BR>** Limit of 5/user **|
| caracol | PI | yes | (none) | (none) | (none) | (none/90d) | Lieber |
| neuron | PI | yes | (none) | (none) | (none) | (none/90d) | |
| bstgpu | dept | yes | (none) | (none) | (none) | (none/90d) | bst=biostatistics |
| bstgpu2 | dept | yes | (none) | (none) | (none) | (1d/7d) | bst=biostatistics |
| bstgpu3 | dept | yes | (none) | (none) | (none) | (none/90d) | bst=biostatistics |
