---
tags:
  - needs-to-be-written
  - slurm
  - jeffrey
---

# Introduction to SLURM

Until we write this document, please

1. see the latest [orientation document (pdf)](../orient/images/latest-orient.pdf)
2. check out other SLURM-related pages on this web site. There is already some great information here. The [commands tip and reference](../slurm/slurm-commands-ref.md) document contains links to both manual pages and pages we've written with guidance and examples.
3. If you can't find what you are looking for here, we have a collection of good documentation from other clusters [here](../slurm/user-guide-collection.md). SLURM is a popular tool, so Google searches will also be productive.

## The Basics
(Needs to be written)

## Defaults
By default, if you do not request any specific resources, your job will receive:

- 1 CPU
- 5G of memory
- 1 day of run time (on public [partitions](../slurm/partitions.md))
- assignment to the `shared` partition.

## Jobs 

Please visit the "About SLURM Jobs" document [here](../slurm/about-jobs.md) to read about basic SLURM facts like job notation, steps, names and states.

Jobs can be submitted on any SLURM node, although usually this is done on one of the login nodes.  If the job is accepted, it is assigned a unique number. When the scheduler starts a job running, the job will be assigned one or more nodes (depending on the job submission directives). The first node is called "node zero" for the job. All of the nodes associated with a job are contained in a "nodelist" 
