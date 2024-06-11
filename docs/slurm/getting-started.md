---
tags:
  - slurm
---

# Introduction to SLURM

See the latest [orientation document (pdf)](../orient/images/latest-orient.pdf) for examples of how to use SLURM to run jobs.

Please check out other SLURM-related pages on this web site. There is already some great information here. 

## Defaults
By default, if you do not request any specific resources, your job will receive:

- 1 CPU core (with support for two hyperthreads)
- 5G of memory
- 1 day of run time (on public [partitions](../slurm/partitions.md))
- assignment to the `shared` partition.

## The Basics
The Slurm Workload Manager, or simply Slurm, formerly known as Simple Linux Utility for Resource Management (SLURM), is a free and open-source job scheduler for UNIX operating systems (Linux is a branch of UNIX). We've continued to capitalize the name in our documentation at JHPCE to help keep the term more visible.

Job schedulers like SLURM perform a number of functions. At heart, they allow people to manage computing work across many compute nodes.

- Tasks are called **_jobs_**.
- Jobs can be submitted on any SLURM node, although usually this is done on one of the ***login nodes***.  If the job is accepted, it is assigned a unique number, the ***jobid***.
- Jobs can be ***interactive***, (run in real time), or **_batch_**, which are scheduled for future unattended execution
- Jobs specify resources that they will need, such as the number of CPU cores, amount of RAM (in megabytes) and job duration
- Jobs are submitted to queues or **_partitions_** (collections of compute nodes governed by specific rules (such as priorities))
- Jobs are assigned to ***compute nodes*** as the CPU and RAM resources required become available in sufficient quantities
- The dispatch of jobs from a pending status to running on compute node(s) can be governed by a number of policy tools, such as [priorities](../slurm/whenstart.md) and [Quality of Service](../slurm/qos.md)

Please visit the [About SLURM Jobs](../slurm/about-jobs.md) document to read about basic SLURM facts like job notation, steps, names and states.

## SLURM Commands and Tips For Using Them

The [commands tip and reference](../slurm/slurm-commands-ref.md) document describes programs and their uses, whether they were written by the vendor or JHPCE staff. That page contains links to both manual pages and pages we've written with guidance and examples.

If you can't find what you are looking for here, we have a collection of good documentation from other clusters [here](../slurm/user-guide-collection.md). SLURM is a popular tool, so Google searches will also be productive. Just keep in mind that the documentation for other clusters will include information specific to their hardware, software and policies.

## Transition from Sun Grid Engine to SLURM

We transitioned from the Sun Grid Engine to SLURM in 2024. We wrote [this document](../orient/images/transition-sge-2-slurm.pdf) for users familiar with SGE who needed to learn some SLURM basics.
