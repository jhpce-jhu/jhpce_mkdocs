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


## Jobs 

If you are curious, the SLURM vendor describes how jobs are started and terminated in [this document](https://slurm.schedmd.com/job_launch.html).

Jobs can be submitted on any SLURM node, although usually this is done on one of the login nodes.  If the job is accepted, it is assigned a unique number. When the scheduler starts a job running, the job will be assigned one or more nodes (depending on the job submission directives). The first node is called "node zero" for the job. All of the nodes associated with a job are contained in a "nodelist" 


Job notation 

```
<job_array>_<job_id>.<step_id>
```

### Jobs Have Multiple Steps

You will notice in the output of `sacct` commands that each job has multiple entries, including one for the overall job. Each entry has a STATE code (e.g. COMPLETED, FAILED, CANCELLED) and an EXITCODE (e.g. 0:0).

The average job will have three steps -- one each of these types: 

* **extern step** - the connection from the node on which you submitted the job and the leading node of your nodelist. This step normally succeeds whether or not your overall job does.
* **batch step** - created for jobs submitted with `sbatch`  The exit code of this script impacts the final STATE of this step.
* **normal step** - a job can have multiple normal steps, which will appear in accounting output as `<job_id>.<step_id>`


