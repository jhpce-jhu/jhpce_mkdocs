---
tags:
  - in-progress
---

# Copying Files Within The Cluster



## Rsync

Rsync is a powerful command for copying large numbers of files between a SOURCE and a DESTINATION (aka DEST), within a single computer or between computers. It provides MANY arguments to control how this is done. A key benefit of rsync is that it will copy only the material needed to make DESTINATION match SOURCE. Most other programs, such as cp or scp, will always copy over all of SOURCE to DESTINATION.

### Mind the trailing slash

### Rsync Examples

### Rsync Flags You May Want To Use

### Example batch job

Is shown [here](../slurm/crafting-jobs.md#copying-data-within-cluster).
