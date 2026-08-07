---
tags:
  - slurm
---
# Factors Affecting Job Scheduling

## TL;DR

Request the smallest and/or fewest resources which will allow your job to run to completion.

Choose the appropriate partition for your work. Partitions have different rules, which are explained [here](../slurm/partitions.md).

Smaller jobs "fit" into more slots between other jobs than larger jobs, so consider whether you can divide up your work. You should also direct your job to the most appropriate partition. For example, the JHPCE cluster has an "interactive" partition for small jobs.

Understand that we have implemented various policies which impact the ordering of jobs of the same "size".

(In the JHPCE cluster:) If you have used a lot of resources in public partitions recently, (in the last week, especially over the last three days), your jobs will have a lower _priority_ than those of someone who has consumed less.

If the scheduler has been able to determine an estimated start date for your job, it will be shown in the output of

```Shell title="Start time estimate" linenums="0"
squeue --me --start
```

## Overview - The Problem

A job consists of at least three resources:<br>

* maximum job duration (**time**),<br>
* maximum amount of memory (**RAM**), and<br>
* maximum number of CPU cores (**CPU**).

Other resources might include a partition, a device like a GPU card, or a reservation.

You can think of the total of these resource requests as a multiple dimensioned box of a certain "size" defined by adding together the components. Then visualize looking at a chart where you have to fit each box into your collection of compute or GPU nodes, at a specific time.

One of SLURM's primary functions is to schedule jobs, which consists of two tasks:<br>

* **Technical**: Finding the right "sized" possible slots for each job either now or into the future. The "smaller" the slot, the easier it will be to find.
* **Policy**: Choosing specific jobs to place into specific slots according to other factors chosen by cluster administrators.

Over the decades, organizations using tools like SLURM have desired both<br>

* the highest efficiency utilization of available hardware, and<br>
* the ability to set policies about who gets which resources, and in what order.


## Our Choices

SLURM has many "knobs" that can be tuned to try to reach those goals. Some of the most important ones are described below. Our clusters (currently JHPCE and JADE) contain compute node hardware purchased, in the main, by primary investigators. Most of them choose to share their nodes with the larger community because they value collaboration and doing so reduces the costs of operating their nodes. We group nodes together into SLURM partions, each of which can have different rules applied to them. Those rules are part of implementing policies, such as giving members of a PI's research teams better access to the hardware that team paid for.  As a general rule, this has resulted in having two sets of partitions: "private" ones for PI teams and "public" ones for everyone to use. (Details about partitions can be found at [here](../slurm/partitions.md].)

### Resource Limits

Public partitions have rules in place which restrict the amount of each of the main types of job resources. For example, JHPCE's "shared" partition's definition includes a maximum job duration as well as a Quality of Service (QOS are described [here](../slurm/qos.md). The QOS for shared limits the amount of RAM and CPUs that a user can consume at any one time.

### Priorities

We are using a common one, which is enabling(but they often have unintended consequences!).  level decision is to 

 such as "fairness". That can mean ensuring that heavy recent consumers of resources receive a lower priority than folks who have just arrived in the community.

Running jobs only in the order that they are submitted on nodes in hostname order is one approach the scheduler could follow, but it turns out to be inefficient. That also doesn't allow organizations to implement policies to favor some jobs over others. So schedulers like SLURM have incorporated many features over the decades which help make maximum use of the cluster's resources and implement these other goals.

so they run on nodes which have the resources needed.
What are some of the things that go into these decisions?

How does it choose which of a set of pending jobs to start in what order on which node and which CPU cores on the node?

There are a number of vendor documents which document scheduling. See the "Workload Prioritization" and "Slurm Scheduling" sections at this [site](https://slurm.schedmd.com/documentation.html).




## Backfill

In addition to the main scheduling cycle, where jobs are run in the order of priority and availability of resources, all jobs are also considered for "backfill". Backfill is a mechanism which will let jobs with lower priority score start before high priority jobs if they can fit in around them. For example, if a higher priority job needs 30 cores and it will have to wait 20 hours for those resources to be available, if a lower priority job only needs a couple cores for an hour, Slurm will run that shorter job in the meantime. This GREATLY enhances utilization of the cluster.

For this reason, **it is important to request accurate walltime limits for your jobs**. If your job only requires 2 hours to run, but you request 24 hours, the likelihood that your job will be backfilled is greatly lowered. 


## Priority

!!! Caution "Cluster-specific"
    As of 20240320, multifactor priority is not enabled on the C-SUB.
    
Multiple factors are used to assign a single priority value to each job. This is described in the [Multifactor Job Priority](https://slurm.schedmd.com/priority_multifactor.html) document.

(This priority is only used to decide which jobs to dispatch first. It is not used to set a UNIX process `nice` value on the processes created by jobs out on the compute nodes.)

Once a job starts running, its priority no longer has much meaning.

The job's priority is an integer that ranges between 0 and 4,294,967,295. The larger the number, the higher the job will be positioned in the queue, and the sooner the job will be scheduled and started. A job's priority, and hence its order in the queue, can vary over time.

The final priority is determined by multiplying pairs of (weights and factors) and adding the results. Factors range from 0.0 to 1.0. Weights range from 0 to 65,533.

Currently we are using three components: [Age](https://slurm.schedmd.com/priority_multifactor.html#age), [Fairshare](https://slurm.schedmd.com/priority_multifactor.html#fairshare) and [Partition](https://slurm.schedmd.com/priority_multifactor.html#partition)

!!! Tip
    You can see pending job's priority values and the contributors to the final value with the `sprio` command. This sorts jobs by total prio, partition, user.
    ```Shell title="Pending jobs sorted by priority" linenums="0"
    sprio -S -y,p,u | less
    ```

!!! Tip
    A better formatted of that command which prints only the factors we are currently using[^1] is:

    [^1]: This command's output will be incomplete if we begin using other priority factors.

    ```Shell title="Pending jobs sorted by priority, well-formatted" linenums="0"
    sprio -o "%.15i %9r %.8u %.10Y %.10A %.10F %.10P" -S -y,p,u
    ```
!!! Tip
    You can change the priority of your jobs among your jobs with `scontrol` commands like `top` and `nice`. See [this document](tips-scontrol.md) for details. 
### Fairshare
To help provide equitable access to the public partitions of the cluster, the FAIRSHARE priority component is based on your recent usage of those partitions. If you have used fewer CPU minutes than someone else in the last week, then your jobs will receive a higher fairshare value.

The fairshare priority is the result of multiplying a weight stored in a variable, PriorityWeightFairshare, and a factor which is derived from the accounting database.

!!! Tip
    You inspect fairshare values for ALL users with this command:

    ```Shell title="Fairshare values" linenums="0"
    sshare -a | sort -k7nr | less
    ```

You should focus on the values in the right-hand-most column. Heaviest users of the cluster in recent days have values closer to 0.0. People who haven't run any jobs lately will have values closer to 1.0. Jobs submitted by the latter will be given higher fairshare priority values.

### Age

In addition to fairshare, any pending job will accrue AGE priority over time. Currently (20240217) this maxes out to 100 over the course of a week.

Job arrays which started running tasks many days ago will wind up with high age priority values for all of their future tasks. You can see that fairshare somewhat counteracts that age advantage.

If you decide that you want to change something about a pending job, consider whether you can do so using `scontrol` commands as described [here](tips-scontrol.md) instead of killing the job with `scancel` and resubmitting it. That would preserve the age priority your job has accrued.

### Partition Priority

We have set this experimentally on the `interactive` partition to try to aid in quick access to (small) interactive sessions.
