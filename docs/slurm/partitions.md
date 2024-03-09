---
tags:
  - slurm
---


# Partitions

A partition is a logical collections of nodes that comprise different hardware resources and limits to help meet the wide variety of jobs that get scheduled on the cluster. 

There are several types of partitions:

* **General access** (e.g. shared, interactive, gpu, transfer)
* **Application only** (e.g. sas)
* **GPU** (equipped with GPU cards)
* **PI-owned** (for use only by members of the PI's group)

## PI Partitions
{==Only submit jobs to these partitions if you are a member of the Primary Investigator's research groups or have been given explicit permission to do so.==} If you are in doubt, ask before submitting. Jobs from non-group members will be killed and repeated abuse *will* lead to repercussions.  

## Public Partitions 

Partitions **shared**, **interactive**, **gpu** and **sas** are considered public and available to all.

Only jobs which require the use of GPU cards should be submitted to the **gpu** partition.

Only jobs which require the use of the SAS application should be submitted to the **sas** partition.

The public partitions provide low-priority access to unused capacity throughout the cluster. Capacity on the shared queue is provided on a strictly “as-available” basis and serves two purposes.

First it provides surge capacity to stakeholders who temporarily need more compute capacity than they own, and second, it gives provides non-stakeholders access to computing capacity.

Scheduling polices attempt to harvest unused capacity as efficiently as possible while mitigating the impact on the rights of stakeholders to use their resources. The JHPCE service center does not guarantee that stakeholders will provide sufficient excess capacity to meet non-stakeholders needs, however in practice the cluster is rarely operating at full capacity so there is usually ample computing capacity on the shared queue.

## Getting Info About Partitions

The command [`scontrol`](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html) can be used to show information about a specific partition. This is THE best way to see the configuration of a partition. The syntax to use is
```bash linenums="0"
`scontrol show partition` *partitionname*
```

The command [`sinfo`](https://slurm.schedmd.com/archive/slurm-22.05.9/sinfo.html) shows information about all of the partitions. (Note: Partitions which *require* group membership to submit to are only visible via `sinfo` to members of those groups. Because the local command `slurmpic` uses `sinfo` to retrieve information, the output of `slurmpic -a` (show all nodes) will omit those private PI partitions' nodes.)

## CPU Partitions

### Public CPU Partitions

To reduce table width, column names are terse.

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Approval[1] | Core | RAM | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- |
| shared | public | no | 100 | 1TB | (1d/90d) | DEFAULT |
| interactive | public | no | 2 | 20gb | (1d/90d) | Small but accessible |
| gpu | public | no | (none) | (none) | (1d/90d) | Only for GPU jobs |
| sas | application | no | (none) | (none) | (none/90d) | Licensed for SAS |

### PI CPU Partitions

| Name | Type | Approval[1] | Core | RAM | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- |
| bader | PI | yes | (none) | (none) | (none/90d) | |
| bluejay | PI | yes | (none) | (none) | (none/90d) | UNIX group |
| cancergen | PI | yes | (none) | (none) | (none/90d) | |
| caracol | PI | yes | (none) | (none) | (none/90d) | UNIX group |
| cee | PI | yes | (none) | (none) | (none/90d) | |
| cegs2 | PI | yes | (none) | (none) | (none/90d) | |
| chatterjee | PI | yes | (none) | (none) | (none/90d) | |
| echodac | PI | yes | (none) | (none) | (none/90d) | |
| gwas | PI | yes | (none) | (none) | (none/90d) | not yet defined |
| hl | PI | yes | (none) | (none) | (none/90d) | |
| hpm | PI | yes | (none) | (none) | (none/90d) | not yet defined |
| hongkai | PI | yes | (none) | (none) | (none/90d) | |
| mommee | PI | yes | (none) | (none) | (none/90d) | |
| stanley | PI | yes | (none) | (none) | (none/90d) | UNIX group |
| sysadmin | admin | yes | (none) | (none) | (none/90d) | For system testing |
| transfer | public | no | (none) | (none) | (none/90d) | Data in or out of cluster via SLURM jobs |

[1] Requires approval of PI before submitting jobs.

## GPU Partitions
Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Requires Approval | Core | RAM | GPU | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- | ------|
| gpu | public | no | (none) | (none) | (none) | (1d/90d) | |
| bestgpu | PI | yes | (none) | (none) | (none) | (none/90d) | |
| caracol | PI | yes | (none) | (none) | (none) | (none/90d) | |
| neuron | PI | yes | (none) | (none) | (none) | (none/90d) | |
