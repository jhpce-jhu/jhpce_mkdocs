---
tags:
  - in-progress
  - slurm
---
!!! Warning
    This page is just getting started. (Insert the sounds of heavy construction equipment here.)
    
# **All About SLURM Jobs**
Well, maybe not ALL.

Here we try to describe some basic facts about jobs. We will point to this document in the other places in the web site where we mention some of these core facts.

SLURM is popular because it is flexible and supports many different computational needs. Users do complex things on various clusters, and the way you normally use SLURM may not at all match the way someone else does.


## **WHAT IS A SLURM JOB?**
That's a good question. Kind of philosphical.

A job consists of
1. an optional set of directives
2. one or more steps.

## **JOB NOTATION** 

Once jobs are submitted and accepted, they are given a **job id** number.
When specifying a job to a SLURM program like `scontrol` or reading output from a SLURM program like `sacct`, you will see a variety of forms of identification numbers.

Underscores ++underscore++ divide job_arrays from job_ids and periods ++period++ divide job_ids from step_ids.

Sometimes you can use a job_array number where the documentation says job_id, as when cancelling the whole job array with `scancel` 

```
<job_id>
<job_id>.<step_id>
<job_array>_<job_id>
<job_array>_<job_id>.<step_id>
```

## **JOB HAVE MULTIPLE STEPS**

You will notice in the output of `sacct` commands that each job has multiple entries, including one for the overall job. Each entry has a STATE code (e.g. COMPLETED, FAILED, CANCELLED) and an EXITCODE (e.g. 0:0). Keep that in mind when viewing state information. 

The average job will have three steps -- one each of these types: 

* **extern step** - the connection from the node on which you submitted the job and the leading node of your nodelist. This step normally succeeds whether or not your overall job does.
* **batch step** - created for jobs submitted with `sbatch`  The exit code of this script impacts the final STATE of this step.
* **normal step** - a job can have multiple normal steps, which will appear in accounting output as `<job_id>.<step_id>`




## **JOB STATES**

If you are curious, the SLURM vendor describes how jobs are started and terminated in [this document](https://slurm.schedmd.com/job_launch.html).

Until we draw a nice diagram, a written description will have to suffice. Here we focus on batch jobs and common states. Job states have short names consisting of one or two capitalized letters, and a full name. Short names are introduced in parentheses below. We have placed a copy of the list in `/jhpce/shared/jhpce/slurm/docs/job-states.txt`.

??? Note "Click for a complete list of job states"
    --8<-- "slurm/images/job-states.md"

The main job states you will see:

|Short|Long|
|-----|----|
|PD|PENDING|
|R|RUNNING|
|CD|COMPLETED|
|F|FAILED|
|CA|CANCELLED|

Batch jobs consist of several job steps, two at minimum ("batch"[^2] and "extern"[^3]). The overall job and each step has its own job state code. They often differ. The `-X` flag to sacct will show you only the overall job state, such as FAILED, but some times you need to check the state of all of a jobs steps in order to see that a "batch" step ran OUT_OF_MEMORY.

[^2]: The batch script that you have created to run your commands.
[^3]: The external step is the connection SLURM makes to the compute node to begin executing your job.

1. User submits job
2. SLURM evaluates syntax and resource requests.
3. If problems found, then job is rejected immediately.
4. Otherwise it is accepted and becomes PENDING (PD).
5. SLURM's scheduler algorithms begin testing where your job will "fit" in its planning process for the future. There will be more slots where smaller jobs (in RAM, CPU and duration) will fit than larger ones.
5. PENDING jobs can be held, CANCELLED (CA) or be dispatched to compute node(s) to start RUNNING (R).
6. RUNNING jobs can immediately run into a problem due to coding errors and become FAILED (F).
7. RUNNING jobs can run correctly but then exceed their memory allocation and become OUT_OF_MEMORY (OOM).
8. RUNNING jobs can run correctly but run into their wall-clock time limit and become DEADLINE (DL) or FAILED.
9. RUNNING jobs can run correctly, switch to COMPLETING (CG) as processes are quitting, and then COMPLETED (CD).


