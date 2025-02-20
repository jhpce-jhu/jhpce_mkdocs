---
tags:
  - slurm
---


# Partitions

A partition is a logical collections of nodes that comprise different hardware resources and limits to help meet the wide variety of jobs that get scheduled on the cluster. Nodes can belong to more than one partition.

There are several *types* of partitions:

* **General access** (e.g. shared, interactive, gpu, transfer)
* **Application only** (e.g. sas)
* **GPU** (equipped with GPU cards)
* **PI-owned** (for use only by members of the PI's group)

## Choosing Partitions For Your Jobs
You should only submit jobs to partitions that you are entitled to use.

Jobs can be submitted to multiple partitions to increase the odds that they will start more quickly. They will generally start in the first partition that has the required resources. This is mainly of use to members of PI partitions where their PI partition member nodes are busy at the moment, or they need maximum resources to meet a deadline.

Simply separate partition names with commas (and no spaces). For example, in a batch job file:

```
`#SBATCH --partition=cancergen,shared`
```
 
There are a number of ways in which partitions are configured which may nudge a pending job to one partition over others. Pending jobs in some partitions are evaluated ahead of others. Priorities can be set on some partitions (such as `interactive`) as described [here](../slurm/whenstart.md/#partition-priority).

Array jobs *seem* to be able to dispatch child jobs to any of the partitions specified. We are not 100% sure of this -- they might all be run into the first partition chosen. (If the latter is true, you may not want to tie your array completion to a partition that has many fewer resources than your other option(s).)

## PI Partitions
JHPCE exists because Primary Investigators worked together to create a cluster. They share their resources via public partitions (see cost recovery descriptions [here](../aboutus/model.md/#cost-recovery) and [here](../joinus/new-pi.md)).

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

Our command `slurmpic` shows information about partitions, including the member nodes, their current utilization, and some summary statistics.[^1] Text is color-coded to try to indicate how fully consumed nodes are. By default it displays the **shared** partition. Specific partitions can be displayed using `slurmpic -p partitionname`. All of the nodes in all of the GPU partitions can be displayed with `slurmpic -g`. Individual GPU-containing partitions can be shown with `slurmpic -g -p partitionname` {==Important:==} Run `slurmpic -h` to see important usage notes!

[^1]: Note that the statistics displayed are for that partition, not the whole cluster. Also, memory and CPU use of nodes that are DOWN or in DRAIN are not included in the stats.
 
The best way to see the _configuration_ of a partition is with the scontrol command. 
```bash linenums="0"
scontrol show partition partitionname
```
(For tips about using `scontrol`, see our [local scontrol tips](../slurm/tips-scontrol.md) page.)

`slurmpic` is based on the SLURM command [`sinfo`](https://slurm.schedmd.com/archive/slurm-22.05.9/sinfo.html), which shows information about nodes and partitions. It has many options, so you can also use it to see information about nodes. (Note: Partitions which *require* group membership to submit to are only visible via `sinfo` to members of those groups. Because the local command `slurmpic` uses `sinfo` to retrieve information, the output of `slurmpic -a` (show all nodes) will omit those private PI partitions' nodes.)

## CPU Partitions

### Public CPU Partitions

The CPU and RAM limits for `shared` change depending on node availability. We change the QOS `shared-default` up and down based on usage over the past month or two. We try to keep limits as high as possible to increase the utilization rate of the cluster while not allowing one person to dominate the `shared` partition. You can learn more about QOS [here](../slurm/qos.md) and see the currently defined QOS values with the command `showres`

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Core | RAM | Time | Notes/Use |
| ---- | :----: | ---- | ---- | :-------: | ----- |
| shared | public | 400 | 2.5TB | (1d/90d) | DEFAULT |
| interactive | public | 2 | 20gb | (1d/90d) | Small but accessible |
| interactive-larger | public | 8 | 80gb | (1d/5d) | Limit 2 jobs per user |
| gpu | public | (none) | (none) | (1d/90d) | Only for GPU jobs |
| sas | application | (none) | (none) | (none/90d) | Licensed for SAS |
| scavenge | public | 11 per job | 250G per job | (1d/5d) | See below |
| transfer | public | (none) | (none) | (none/90d) | Data in or out of cluster via SLURM jobs |

To reduce table width, column names are terse.

{==Experimental==} `interactive-larger` partition: This was created to try to allow people to run interactive jobs a bit larger in size than our smaller `interactive` partition. 
{==Experimental==} `scavenge` partition: This was created to try to "harvest" some usually-unused resources from some specific nodes.

### PI CPU Partitions

To see the member nodes and resources of the any partitions, use `slurmpic -p partitionname`

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Core | RAM | Time | Notes/Use |
| ---- | :----: | ---- | ---- | :-------: | ----- |
| bader | PI | (none) | (none) | (none/90d) | |
| cancergen | PI | (none) | (none) | (none/90d) | |
| caracol | PI | (none) | (none) | (none/90d) | UNIX group |
| cee | PI | (none) | (none) | (none/90d) | |
| cegs2 | PI | (none) | (none) | (none/90d) | |
| chatterjee | PI | (none) | (none) | (none/90d) | |
| echodac | PI | (none) | (none) | (none/90d) | |
| gwas | PI | (none) | (none) | (none/90d) | not yet defined |
| hl | PI | (none) | (none) | (none/90d) | hl = hearing loss |
| hpm | PI | (none) | (none) | (none/90d) | not yet defined |
| hongkai | PI | (none) | (none) | (none/90d) | |
| katun | PI | (none) | (none) | (none/90d) | UNIX group |
| mommee | PI | (none) | (none) | (none/90d) | |
| stanley | PI | (none) | (none) | (none/90d) | UNIX group |
| sysadmin | admin | (none) | (none) | (none/90d) | For system testing |


## GPU Partitions
To see the member nodes and resources of the any partitions, use `slurmpic -g`

To learn more about the GPU card types and how to use them, see [https://jhpce.jhu.edu/gpu/gpu-info/](https://jhpce.jhu.edu/gpu/gpu-info/)

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Requires Approval | Core | RAM | GPU | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- | ------|
| gpu | public | no | (none) | (none) | (none) | (1d/90d) | ONLY public GPU resource|
| caracol | PI | yes | (none) | (none) | (none) | (none/90d) | Lieber |
| neuron | PI | yes | (none) | (none) | (none) | (none/90d) | |
| bstgpu | PI | yes | (none) | (none) | (none) | (none/90d) | bst=biostatistics |
| bstgpu2 | PI | yes | (none) | (none) | (none) | (none/90d) | bst=biostatistics |
| bstgpu3 | PI | yes | (none) | (none) | (none) | (none/90d) | bst=biostatistics |
