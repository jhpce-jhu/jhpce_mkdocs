---
tags:
  - slurm
---
    
# **All About SLURM Jobs**
Well, maybe not ALL.

Here we try to describe some basic facts about jobs. We will point to this document in the other places in the web site where we mention some of these core facts.

SLURM is popular because it is flexible and supports many different computational needs. Users do complex things on various clusters, and the way you normally use SLURM may not at all match the way someone else does.


## **WHAT IS A SLURM JOB?**
A ***job*** can span multiple compute nodes and is the sum of all task resources.

A job consists of

1. an optional set of ***directives***
2. a command to run, either bash (for an interactive job) or a shell script[^1] (for a batch job)
3. one or more ***steps***, numbered starting from 0.[^2]

[^1]: One does not have to use the bash shell for interactive sessions or to execute your batch script. You can specify other shells or interpreters.

[^2]: (It is unclear to the author whether an interactive job officially has a step. No steps are listed for them in the sacct output, but he suspects that, technically, in the vendor's documentation, it does have a step. Because tasks are implemented by job steps.)

**Directives** can come from a variety of sources, including, in [order of preference](../slurm/crafting-jobs.md/#slurm-directive-order-of-precendence): on the command line, from environment variables, embedded in a batch file script on `#SBATCH` lines, or from a personal `~/.slurm/defaults` file. If you don't specify a directive, them some default value will be used. Often that default is one, as in one task, one CPU core, one compute node, ...

A job aims to complete one or more ***tasks***.

Tasks are requested at the job level with `--ntasks` or `--ntasks-per-node`, or at the step level with  `--ntasks`. CPUs are requested per task with `--cpus-per-task`.

A **task** is executed on a single ***compute node*** in a job step, using one or more CPU units and a non-zero amount of memory (5 gigabytes if you don't specify -- you can request less than 5G!! -- the less memory your job needs, the more likely it will be able to run).

If there are multiple tasks, then each uses a subset of the resources of the overall job.  Multiple tasks can run on a single compute node or on multiple nodes. The resources used by any one task cannot exceed what is found on a single node.

Jobs execute on a set of one or more compute nodes called a ***nodelist***. The first node in that set is called the ***0th node***.[^3]

[^3]: If there are more than one nodes, one should look on the 0th node for  information about the job in /var/log/slurm/slurmd.log file when troubleshooting.

## **JOB NOTATION** 

Once jobs are submitted and accepted, they are given a **job id** number.
When specifying a job to a SLURM program like `scontrol` or reading output from a SLURM program like `sacct`, you will see a variety of forms of identification numbers.

In our documentation and email, we often say simply "job" or "jobid" with the expectation that you will determine the right value to use in the situation.

Underscores ++underscore++ divide **job arrays** from **job ids**

Periods ++period++ divide **job ids** from **step ids**.

Sometimes you can use a job_array number where the documentation says job_id, as when cancelling the whole job array with `scancel` as opposed to cancelling just one of its job elements.

```
<job id>
<job id>.<step id>
<job array>_<job id>
<job array>_<job id>.<step id>
```

## **JOB HAVE MULTIPLE STEPS**

You will notice in the output of `sacct` commands that each job has multiple entries, including one for the overall job. Each entry has a STATE code (e.g. COMPLETED, FAILED, CANCELLED) and an EXITCODE (e.g. 0:0).  Keep in mind when viewing state information whether you are looking at the overall job state or that of a component.

The average job will have two or more steps of these types: 

* **external step** - the connection from the node on which you submitted the job and the leading node of your nodelist. This step normally succeeds whether or not your overall job does.
* **batch step** - created for jobs submitted with `sbatch`  The exit code of this script impacts the final STATE of this step.
* **interactive step** - created for jobs submitted with `srun` (outside of a batch job)
* **normal step** - a batch job can have multiple normal steps, which will appear in accounting output as `<job_id>.<step_id>` Each step will be created by an `srun` command. Step numbers start at 0. Interactive jobs do not have any normal steps.

### **JOB NAMES**
You can give a job a ***job name***. This explicit name can be used with some commands instead of jobids.

* The name of an *external step* will _always_ be "extern"
* The default overall name of a *batch job* will be "bash"
* The name of a *batch job's normal step(s)* will _always_ be "bash"
* The default overall name of an *interactive job* will be "bash"

The `sacct` command is an important one to know how to use. We have a document explaining how to use it [here](../slurm/tips-sacct.md). Please read it, because **_you need to know_** how to look up information about your jobs. By default it prints fields only 20 characters wide. It will display a ++plus++ when there is more information beyond 20 char, as seen below for the external steps.

```bash title="Example of a batch job"
  JobID           JobName  Partition    Account  AllocCPUS      State ExitCode 
  ------------ ---------- ---------- ---------- ---------- ---------- -------- 

  1922948            bash        cee      jhpce         24    RUNNING      0:0 
  1922948.ext+     extern                 jhpce         24    RUNNING      0:0 
  1922948.0          bash                 jhpce         24    RUNNING      0:0 
```
```bash title="Example of an interactive job (lacks normal step)"
  JobID           JobName  Partition    Account  AllocCPUS      State ExitCode 
  ------------ ---------- ---------- ---------- ---------- ---------- -------- 
  1542874            bash     shared      jhpce          2    RUNNING      0:0 
  1542874.ext+     extern                 jhpce          2    RUNNING      0:0 
```

## **JOB STATES**

If you are curious, the SLURM vendor describes how jobs are started and terminated in [this document](https://slurm.schedmd.com/job_launch.html).

Until we draw a nice diagram, a written description will have to suffice. Here we focus on batch jobs and common states. Job states have short names consisting of one or two capitalized letters, and a full name. Short names are introduced in parentheses below. We have placed a copy of the list in `/jhpce/shared/jhpce/slurm/docs/job-states.txt`.

??? Note "Click for a complete list of job states"
    --8<-- "slurm/images/job-states.md"

The main job states you will see:

|Short Name|Long Name|
|-----|----|
|PD|PENDING|
|R|RUNNING|
|CG|COMPLETING|
|CD|COMPLETED|
|F|FAILED|
|CA|CANCELLED|
|OOM|OUT_OF_MEMORY|
|TO|TIMEOUT|

{==The overall job and each step has its own job state code.==} They often differ!!  The `-X` flag to `sacct` will show you only the overall job state, such as FAILED, but _sometimes you need to check the state of all of a jobs steps_ in order to see that a "batch" step ran OUT_OF_MEMORY.

### LIFE OF A JOB AND ASSOCIATED STATES

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


