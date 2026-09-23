---
tags:
  - needs-to-be-written
  - jeffrey
  - slurm
---
# **squeue -- useful command examples**
   
!!! Warning
    AS OF 09/22/2026 THIS DOCUMENT CONTAINS ONLY A LITTLE USEFUL INFO.
    This document started as a copy of that for the sacct command because formatting output options are similar, etc. It will be pruned and modified when we have time.

[squeue](https://slurm.schedmd.com/archive/slurm-22.05.9/squeue.html) is the primary basic SLURM command used to display information about pending and running jobs. By default it shows a limited amount of information. We hope that this page will help you use the command effectively, by, for example, describing how to request more columns using the `-o` and `-O` formatting options. 

SLURM jobs are always in one "state" or another. Each state has a full name and an abbrieviation what you can use instead when using tools such as `squeue`. Here the relevent two states are `RUNNING` (abbrieviation: `R`) and `PENDING` (abbrieviation: `PD`) (upper case is not required, but we use it here for clarity).

A key piece of information about PENDING jobs is their `Reason`

!!! Example "Simple examples"
    ```Shell title="Show my pending and running jobs" linenums="0"
    squeue --me   # both types
    squeue --me --states=PD  # only PENDING
    squeue --me --nodelist=compute-112
    
          JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
          35862069  sysadmin     bash  tunison  R      40:29      1 compute-112
    ```

!!! Example
    ```squeue --sort=-p,i --states=PD # sort PENDING by partition and job priority
    ```


Adds columns for: amount of CPU, RAM requested as well as time job limit.

jobid,partition,job name,user,state,time run,time limit,node count,cpu,memory,nodelist or reason

!!! Example
    squeue -S u,-t,p -o "%.6P %.8j %.20u %.2t %.10M %.12l %.4D %.4C %.5m %R"
    
    A bash shell routine "mysq" you can add to your .bashrc:
    mysq() { (squeue -O JobArrayID:10" ",username:8" ",partition:7" ",name:10" ",statecompact:3,reason:12" ",reservation,,PendingTime:7" ",tres:20|sed 's/,no.*//' | sed 's/TRES_ALLOC[ ]*$/TRES_ALLOC/') }
    ```

## [EDITING LEFT OFF HERE -- BELOW IS SACCT-SPECIFIC MATERIAL]


Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.
    
    
## sacct basics

1. By default only your own jobs are displayed. Use the `--allusers` or `-a` flag if necessary.
2. You can choose output fields and control their width. 
4. Even the simplest of batch jobs contain multiple "steps" as far as SLURM is concerned. One of them, named "extern" represents the ssh to the compute node on behalf of your job. Job records consist of a primary entry for the job as a whole  as  well as entries for job steps. The [Job Launch](https://slurm.schedmd.com/archive/slurm-22.05.9/job_launch.html#job_record) page has a more detailed description of each type of job step. You may find the `-X` flag helpful to omit clutter.
5. Regular jobs are in the form: **JobID[.JobStep]**
6. Array jobs are in the form: **ArrayJobID_ArrayTaskID**

## Command Options of Note

Check the [man page](https://slurm.schedmd.com/archive/slurm-22.05.9/squeue.html). There are other useful options. Here we show both the short and human-readable flag names, e.g. `-n` and `--name`. When a flag requires an argument, you need to specify it using an equals character, e.g. `--name=fred_type`  When the argument can contain one or more values, separate them with a comma (but no spaces), e.g. `-p shared,cancergen`.

- `-n, --name` *namelist*  accepts one or more names
- `-p, --partition` *partlist* only those in listed partition(s)
- `-t, --states` *statelist*
- `-w, --nodelist` *nodelist* only show jobs currently running on listed nodes
- `-u` *userlist*  only show jobs which ran by this/these users
- `-n`  noheader
- `-p`  <partition>  only those in that partition
- `-P`  parsable2  does not put a | at end of line
- `--delimeter` <char> - use that char instead of | for `-p` or `-P`
- `--units=[KMGTP]` - display in this unit
- `-k` *minimum time* - looking for jobs with time limits in a range
- `-K` *maximum time* - looking for jobs with time limits in a range
- `-q` *qoslist* - list of qos used


## Available fields

Field meanings are explained in [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAF) of the manual page.

```Shell title="What output fields are available?" linenums="0"
sacct -e
```

```Shell title="See all fields for a job" linenums="0"
sacct -o ALL -j <jobid>
```

## Formatting fields

You can put a %NUMBER after a field name to specify how many characters should be printed, e.g.
   
- format=name%30 will print 30 characters of field name right justified.  
- format=name%-30 will print 30 characters left justified.

## Using Environment Variables

You can define environment variables in your shell to reduce the complexity of issuing sacct commands. You can also set these in shell scripts. Command line options will always override these settings.

SACCT_FORMAT

SLURM_TIME_FORMAT

#### Formatting Dates/Times
You can use most variables defined by the STRFTIME(3) system call. [This web page](https://strftime.org) is a starting point, but what SLURM has chosen to implement may not match.

* %a - abbrieviated name of day of the week
* %m - month as decimal, 01 to 12
* %d - day of month as decimal
* %H - hour as decimal in 24-hour notation
* %M - minute as decimal, 00 to 59
* %T - time in 24-hour notation (%H:%M:%S)

```Shell title="Day of week MM-DD HH:MM" linenums="0"
export SLURM_TIME_FORMAT="%a %m-%d %H:%M" 
```
The start and end field widths show below are suitable for the time format shown above.

```Shell title="Resources requested, used" linenums="0"
export SACCT_FORMAT="user,jobid,jobname,nodelist%12,start%-20,end%-20,state%20,reqtres%40,TRESUsageInTot%200"
```

## Output Fields of Interest

These fields are probably the ones you'll want. See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAF) of the manual page for the list and their meaning. Capitalization does not matter; it is used for readability.

- TRES means Trackable RESources, such as RAM and CPUs.
- A number of fields (not listed) are available to tell you on which node a maximum occurred. Similarly there are fields to tell you minimum, average and maximum values for some items.

### Basics
- User
- JobId
- JobName
- Partition
- State
- ExitCode

### Times
- Submit
- Start
- Elapsed - in format [DD-[HH:]]MM:SS
- End

### Nodes
- AllocCPUS
- AllocNodes
- NNodes - number of nodes requested/used
- NodeList - 

### Resources Requested
- ReqTRES # this is what you will be billed for
- ReqNodes
- ReqCPUS

### Resources Consumed
- TRESUsageInTot
- CPUTime - (elapsed)*(AllocCPU) in HH:MM:SS format
- MaxRSS - Max resident set of all tasks in job
- MaxVMSize - Max virtual memory of all tasks in job
- MaxDiskRead - Number bytes read by all tasks in job
- MaxDiskWrite - Number bytes written by all tasks in job

Virtual Memory Size (VMSize) is the total memory size of a job. It includes both memory actually in RAM (the RSS) and parts of executabilities which were not needed to be read in off of disk into RAM. Because, for example, routines in dynamically linked libraries were never called, so those libraries were not loaded. 

RSS - resident set size (RSS) is the portion of memory (measured in megabytes) occupied by a job that is held in main memory (RAM). The rest of the memory required by the job exists in the swap space or file system, either because some parts of the occupied memory were paged out, or because some parts of the executable were never loaded.
