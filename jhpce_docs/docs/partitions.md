# Partitions

A partition is a logical collections of nodes that comprise different hardware resources and limits to help meet the wide variety of jobs that get scheduled on the cluster. 

There are several types of partitions:

* General access (e.g. shared, interactive, gpu)
* Application only (e.g. sas)
* GPU
* PI owned (accessed only by members of the PI's group)

## CPU Partitions

| Name | Type | Requires Approval | Core Limit | RAM Limit | Time Limits (default/max) |
| ---- | ---- | ----------------- | ---------- | --------- | ------------------------- |
| shared | public | no | 100 | 1TB | (none/90d) |
| interactive | public | no | 2 | 20gb | (none/90d) |
| gpu | public | no | (none) | (none) | (none/90d) |
| bluejay | PI | yes | (none) | (none) | (none/90d) |
| cancergen | PI | yes | (none) | (none) | (none/90d) |
| cee | PI | yes | (none) | (none) | (none/90d) |
| cegs | PI | yes | (none) | (none) | (none/90d) |
| chatterjee | PI | yes | (none) | (none) | (none/90d) |
| echodac | PI | yes | (none) | (none) | (none/90d) |
| bluejay | PI | yes | (none) | (none) | (none/90d) |
| bluejay | PI | yes | (none) | (none) | (none/90d) |
| sas | PI | yes | (none) | (none) | (none/90d) |
| stanley | PI | yes | (none) | (none) | (none/90d) |
| sysadmin | admin | yes | (none) | (none) | (none/90d) |

## GPU Partitions
| Name | Type | Requires Approval | Core Limit | RAM Limit | GPU Limit | Time Limits (default/max)| 
| ---- | ---- | ----------------- | ---------- | --------- | --------- |------------------------- |
| gpu | public | yes | (none) | (none) | (none) | (none/90d) |
| bestgpu | PI | yes | (none) | (none) | (none) | (none/90d) |
| caracol | PI | yes | (none) | (none) | (none) | (none/90d) |
| neuron | PI | yes | (none) | (none) | (none) | (none/90d) |