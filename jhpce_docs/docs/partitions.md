# Partitions

A partition is a logical collections of nodes that comprise different hardware resources and limits to help meet the wide variety of jobs that get scheduled on the cluster. 

There are several types of partitions:

* General access (e.g. shared, interactive, gpu, transfer)
* Application only (e.g. sas)
* GPU (equipped with GPU cards)
* PI owned (accessed only by members of the PI's group)

## CPU Partitions

To reduce table width, column names are terse.

Limits for CPU cores, RAM and Time (default/maximum)

| Name | Type | Approval[1] | Core | RAM | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- |
| shared | public | no | 100 | 1TB | (none/90d) | DEFAULT |
| shared-no-r | public | no | 100 | 1TB | (none/90d) | Does not support R |
| interactive | public | no | 2 | 20gb | (none/90d) | Small but accessible |
| gpu | public | no | (none) | (none) | (none/90d) | |
| bluejay | PI | yes | (none) | (none) | (none/90d) | |
| cancergen | PI | yes | (none) | (none) | (none/90d) | |
| cee | PI | yes | (none) | (none) | (none/90d) | |
| cegs | PI | yes | (none) | (none) | (none/90d) | |
| chatterjee | PI | yes | (none) | (none) | (none/90d) | |
| echodac | PI | yes | (none) | (none) | (none/90d) | |
| hl | PI | yes | (none) | (none) | (none/90d) | |
| hongkai | PI | yes | (none) | (none) | (none/90d) | |
| mommee | PI | yes | (none) | (none) | (none/90d) | |
| sas | application | no | (none) | (none) | (none/90d) | Licensed for SAS |
| stanley | PI | yes | (none) | (none) | (none/90d) | |
| sysadmin | admin | yes | (none) | (none) | (none/90d) | For system testing |
| transfer | public | no | (none) | (none) | (none/90d) | Data in or out of cluster via SLURM jobs |

[1] Requires approval of PI before submitting jobs.

## GPU Partitions

| Name | Type | Requires Approval | Core | RAM | GPU | Time | Notes/Use |
| ---- | :----: | :-----: | ---- | ---- | :-------: | ----- | ------|
| gpu | public | yes | (none) | (none) | (none) | (none/90d) | |
| bestgpu | PI | yes | (none) | (none) | (none) | (none/90d) | |
| caracol | PI | yes | (none) | (none) | (none) | (none/90d) | |
| neuron | PI | yes | (none) | (none) | (none) | (none/90d) | |