---
tags:
  - done
  - slurm
---
# sacct useful command examples
    
!!! Example
    ```Shell title="Show my failed jobs between noon and now" linenums="0"
    sacct -s F -o "user,jobid,state,nodelist,start,end,exitcode" -S noon -E now
    ```

[sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html) is a command used to display information about jobs. It has a number of subtleties, such as the time window reported on and the formatting of output. We hope that this page will help you get the information you need.

sacct can be used to investigate jobs' resource usage, nodes used, and exit codes. It can point to important information, such as jobs dying on a particular node but working on other nodes.

sacct will show all submitted jobs but cannot, of course, provide data for a number of fields until the job has finished. Use the [sstat](https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html) command to get information about running programs. "Instrumenting" your jobs to gather information about them can include adding one or more sstat commands to batch jobs in multiple places.

!!! Tip
    Much of the information on this page can be used with `sstat`, but there are differences, particularly in available output fields.

Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.
    
    
## sacct basics

1. By default only your own jobs are displayed. Use the `--allusers` flag if necessary.
2. Only jobs from a certain time window are displayed by default. The window varies depending the arguments you provide. See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAH) of the manual page. It is recommended to always provide start (`-S`) and end (`-E`) times.
3. You can choose output fields and control their width. 
4. Even the simplest of batch jobs contain multiple "steps" as far as SLURM is concerned. One of them, named "extern" represents the ssh to the compute node on behalf of your job. Job records consist of a primary entry for the job as a whole  as  well as entries for job steps. The [Job Launch](https://slurm.schedmd.com/archive/slurm-22.05.9/job_launch.html#job_record) page has a more detailed description of each type of job step. You may find the `-X` flag helpful to omit clutter.
5. Regular jobs are in the form: **JobID[.JobStep]**
6. Array jobs are in the form: **ArrayJobID_ArrayTaskID**

!!! Warning
    Sacct retrieves data from a SQL database. Be careful when creating your sacct commands to limit the queries to the information you need. Narrow the search as much as possible. 
    That database needs to be modified constantly as jobs start and complete, so we don't want it tied up answering sacct queries. If you want to look at a large amount of data in a variety of ways, consider saving the output to a text file and then working with that file.

## Command Options of Note

Check the [man page](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html). There are other useful options.

- `-X`  show stats for the job allocation itself, ignoring steps (try it)
- `-R` *reasonlist*  show jobs not scheduled for given reason
- `-a`  allusers
- `-N` *nodelist*  only show jobs which ran on this/these nodes
- `-u` *userlist*  only show jobs which ran by this/these users
- `--name=`*namelist* - only show jobs with this list of names
- `-n`  noheader
- `-p`  parsable  puts a | between fields and at end of line
- `-P`  parsable2  does not put a | at end of line
- `--delimeter` <char> - use that char instead of | for `-p` or `-P`
- `--units=[KMGTP]` - display in this unit
- `-k` *minimum time* - looking for jobs with time limits in a range
- `-K` *maximum time* - looking for jobs with time limits in a range
- `-q` *qoslist* - list of qos used

## Start and End Times
It is best to use always specify a `-S` start time and a `-E` end time.

Special time words: **today**, **midnight**, **noon**, **now**

now[{+|-}count[seconds(default)|minutes|hours|days|weeks]]

Examples:
: `now-3day`
: `now-2hr`

Valid time formats are:

                   HH:MM[:SS][AM|PM]
                   MMDD[YY][-HH:MM[:SS]]
                   MM.DD[.YY][-HH:MM[:SS]]
                   MM/DD[/YY][-HH:MM[:SS]]
                   YYYY-MM-DD[THH:MM[:SS]]

## Job State Values
Using the `-s <state>` option, you can prune your search by looking for only jobs which match the state you need, such as F for failed. (All of these work: f, failed, F, FAILED)

!!! Warning
    Different steps of a job can have different end states. For example the "extern" step is often COMPLETED when the "batch" and overall steps are FAILED

See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAG) of the manual page, which has also been saved to a text file you can copy for your own reference `/jhpce/shared/jhpce/slurm/docs/job-states.txt`

Primary job states of interest:

- CA CANCELLED
- CD COMPLETED
- F FAILED
- OOM OUT_OF_MEMORY
- PD PENDING
- R RUNNING
- TO TIMEOUT

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

## Exit Error Codes
In addition to the job's "state", SLURM also records error codes. Unfortunately their [Job Exit Codes](https://slurm.schedmd.com/job_exit_code.html) page doesn't provide a meaning for the numerical values.

Error `0:53` often means that something wasn't readable or writable. For example, job output or error files couldn't be written in the directory in which the job ran (or where you told SLURM to put them with a directive).

```
a guide for exit codes:

0 → success
non-zero → failure
Exit code 1 indicates a general failure
Exit code 2 indicates incorrect use of shell builtins
Exit codes 3-124 indicate some error in job (check software exit codes)
Exit code 125 indicates out of memory
Exit code 126 indicates command cannot execute
Exit code 127 indicates command not found
Exit code 128 indicates invalid argument to exit
Exit codes 129-192 indicate jobs terminated by Linux signals
For these, subtract 128 from the number and match to signal code
Enter kill -l to list signal codes
Enter man signal for more information
```

## Diagnostic Arguments

These can be useful to double-check what someone actually did.

```Shell title="See the full command issued to submit the job" linenums="0"
sacct -o SubmitLine -j <jobid>
```

```Shell title="See batch file used" linenums="0"
sacct -B -j <jobid>
```

```Shell title="Directory used by the job to execute commands" linenums="0"
sacct -o WorkDir -j <jobid>
```

```Shell title="See jobs given a time limit btwn 1min & 1 day" linenums="0"
sacct -k 00:01 -K 1-0
```
