---
tags:
  - done
---
# Factors Affecting Job Scheduling

## Overview

One of SLURM's primary functions is to schedule jobs so they run on various nodes. Running jobs only in the order that they are submitted on nodes in hostname order is one approach the scheduler could follow, but it turns out to be inefficient. That also doesn't allow sites to implement policies to favor some jobs over others. So schedulers have incorporated many features over the decades which help make maximum use of the cluster's resources and implement other goals.

What are some of the things that go into these decisions?

How does it choose which of a set of pending jobs to start in what order on which node and which CPU cores on the node?

## Priority

Multiple factors are used to assign a single priority value to each job. This is described in [this document](https://slurm.schedmd.com/priority_multifactor.html). 

(This priority is only used to decide which jobs to dispatch first. It is not used to set a UNIX process `nice` value on the processes created by jobs out on the compute nodes.)

Once a job starts running, it no longer has a priority.

The job's priority is an integer that ranges between 0 and 4,294,967,295. The larger the number, the higher the job will be positioned in the queue, and the sooner the job will be scheduled and started. A job's priority, and hence its order in the queue, can vary over time.

The final priority is determined by multiplying pairs of weights and factors and adding the results. Factors range from 0.0 to 1.0. Weights range from 0 to 65,533.

Currently we are using two factors. 

You can see pending job's priority values and the contributors to the final value with the `sprio` command. This sorts jobs by total prio, partition, user.

`sprio -S -y,p,u | less`

A better formatted of that command which prints only the factors we are currently using[^1] is:

[^1]: This command's output will be incomplete if we begin using other priority factors.

`sprio -o "%.15i %9r %.8u %.10Y %.10A %.10F %.10P" -S -y,p,u`

### Fairshare
To help provide equitable access to the cluster, the FAIRSHARE priority component is based on your recent usage. If you have used fewer CPU minutes than someone else in the last week, then your jobs will receive a higher fairshare value.

The fairshare priority is the result of multiplying a weight stored in a variable, PriorityWeightFairshare, and a factor which is derived from the accounting database.

You inspect fairshare values for ALL users with this command:
`sshare -a | sort -k7nr | less `

You should focus on the values in the right-hand-most column. Heaviest users of the cluster in recent days have values closer to 0.0. People who haven't run any jobs lately will have values closer to 1.0. Jobs submitted by the latter will be given higher fairshare priority values.

### Age

In addition to fairshare, any pending job will accrue AGE priority over time. Currently (20240217) this maxes out to 100 over the course of a week.

Job arrays which started running tasks many days ago will wind up with high age priority values for all of their future tasks. You can see that fairshare somewhat counteracts that age advantage.

## Backfill

In addition to the main scheduling cycle, where jobs are run in the order of priority and availability of resources, all jobs are also considered for "backfill". Backfill is a mechanism which will let jobs with lower priority score start before high priority jobs if they can fit in around them. For example, if a higher priority job needs 30 cores and it will have to wait 20 hours for those resources to be available, if a lower priority job only needs a couple cores for an hour, Slurm will run that shorter job in the meantime. This GREATLY enhances utilization of the cluster.

For this reason, it is important to request accurate walltime limits for your jobs. If your job only requires 2 hours to run, but you request 24 hours, the likelihood that your job will be backfilled is greatly lowered. 
