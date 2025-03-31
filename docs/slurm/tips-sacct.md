---
icon: material/eye
tags:
  - slurm
---
# **sacct useful command examples**
 
!!! Caution
    If you have not already read about basic job facts like job notation, steps, names and states, please first visit the "About SLURM Jobs" document [here](../slurm/about-jobs.md).
 
## **Sacct Overview**

The [sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html) command is used to query the SLURM job accounting database, usually for jobs which have _ended_ (one way or the other). It has a number of subtleties, such as the formatting of output. `sacct` can be used to investigate jobs' resource usage, nodes used, and exit codes. It can point to important information, such as jobs dying on a particular node but working on other nodes[^1].

[^1]: In which case you can add the directive `--exclude=compute-xxx` to your job submission, then notify us via bitsupport@lists.jh.edu so we can fix that node.

The [sstat](https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html) program is aimed at those who are looking for information about _running_ jobs. Much of the information on this page can be used with `sstat`, but there are differences, particularly in available output fields (compare the output of `sacct -e` and `sstat -e`).

??? Caution "You can use `sstat` to **profile the resource use** of or **instrument** your jobs over their lifetimes."
    You can do that in a variety of ways: (a) interactively, (b) from within your batch job scripts (although not while some other command is running), adding one or more `sstat` commands to the batch script in multiple places, and (c) from other batch jobs that you craft. (You could specify a very small resource job so it launches immediately, (perhaps using the `interactive` partition), learn how to create a hetrogenous job that spawns both the sstat commands in a timed loop as well as running your real program(s), or maybe reads files you create in the main batch job in order to figure out what jobid sstat should be monitoring.
    
!!! Example
    ```Shell title="Show my failed jobs between noon and now" linenums="0"
    sacct -s F -o "user,jobid,state,nodelist,start,end,exitcode" -S noon -E now
    
    User JobID             State        NodeList               Start                 End ExitCode 
    --------- ------------ ---------- --------------- ------------------- ------------------- -------- 
    tunison 14084984         FAILED     compute-112 2025-02-19T14:30:46 2025-02-19T15:35:09      0:9 
            14084984.ex+  COMPLETED     compute-112 2025-02-19T14:30:46 2025-02-19T15:35:09      0:0 
            14084984.0   CANCELLED+     compute-112 2025-02-19T14:30:46 2025-02-19T15:35:09      0:9 

    ```
More examples can be found throughout this document, as well as [at the end](#examples).

[sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html) is a command used to display information about jobs.



Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.


## **sacct basics**

1. By default only your own jobs are displayed. Use the `--allusers` or `-a` flag if necessary.
2. Only jobs from a certain time window are displayed by default. That window varies in a confusing manner depending the arguments you provide. See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAH) of the manual page. Therefore it is recommended to **always** provide start (`-S`) and end (`-E`) times to be sure that you are seeing what you expect.
3. You can choose output fields and control their [width](#formatting-fields). 
4. Even the simplest of batch jobs contain multiple "steps" as far as SLURM is concerned. One of them, named "extern" represents the ssh to the compute node on behalf of your job. Job records consist of a primary entry for the job as a whole  as  well as entries for job steps. The [Job Launch](https://slurm.schedmd.com/archive/slurm-22.05.9/job_launch.html#job_record) page has a more detailed description of each type of job step. You may find the `-X` flag helpful to omit clutter.
5. Regular jobs are in the form: **JobID[.JobStep]**
6. Array jobs are in the form: **ArrayJobID_ArrayTaskID**
7. Jobs have multiple steps!! (Explained [here](../slurm/about-jobs.md#jobs-have-multiple-steps).)

!!! Warning
    Sacct retrieves data from a SQL database. Be careful when creating your sacct commands to limit the queries to the information you need. Narrow the search as much as possible. 
    That database needs to be modified constantly as jobs start and complete, so we don't want it tied up answering sacct queries. If you want to look at a large amount of data in a variety of ways, consider saving the output to a text file and then working with that file.

## **Command Options of Note**

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

### **Sorting or processing output**
`sacct` can output with other delimiters if you specify either `-p` or `-P` and `--delimiter=<characters>`

`sacct` does not have a sort option. You need to sort its output by other methods. If you want to change the order of the information, specify the field names in the desired order. Remember that if you do not include the `-X` option to `sacct`, then the number of output fields might vary between the first of several lines per job and later lines. For example if you request an output format starting with "user", that field is only printed for the first line per job. (shake fist!)

This is a prime example of where it is kindest to the SLURM master daemon if you run your query such that you store the results in a text file, then work with the text file contents, rather than running many variations of a command pipeline beginning with `sacct` when you already have all of the desired output option fields and are just trying to figure out the right text-processing logic to deal with the output using sort or awk or what have you. OR do your initial searching in order to produce the right kinds of output using limited date or other ranges.


```shell title="sort failed jobs by exitcode"
sacct -X -a -s F -o "user,jobid,state,nodelist,exitcode" -S noon -E now|sort -k5| less
```

```shell title="sort failed jobs by exitcode, then by nodelist"
sacct -X -a -s F -o "user,jobid,state,nodelist,exitcode" -S noon -E now|sort -k5,5 -k4,4| less
```

Useful sort options:

1. `-kKEYDEF` or `--key=KEYDEF` Given just a number as the KEYDEF, it sorts by column
2. `-k5,5` Repeating the column number prevents multiple words within a column from being considered extra columns (at least the author thinks that is what is going on) (maybe there is a more sophisticated version of KEYDEF that can also prevent that issue)
2. `-r`or `--reverse`
3. `-n` or `--numeric-sort` 

See the sort manual page for other options, including multiple kinds of numeric sorts, as well as the syntax of KEYDEF.

## **Start and End Times**
{==It is best to use always specify a `-S` start time and a `-E` end time.==}

Special time words: **today**, **midnight**, **noon**, **now**

Positive and negative deltas from now can be used, which is very handy, but {==remember to use the full unit word==} (e.g. "days" not "d" or "day")!!!

now[{+|-}count[seconds(default)|minutes|hours|days|weeks]]

Examples:
: `now-3days`
: `now-2hours`

Valid time formats are (spare brackets indicate optional elements):

                   HH:MM[:SS][AM|PM]
                   MMDD[YY][-HH:MM[:SS]]
                   MM.DD[.YY][-HH:MM[:SS]]
                   MM/DD[/YY][-HH:MM[:SS]]
                   YYYY-MM-DD[THH:MM[:SS]]

Examples:
: 0408  # April 8th
: 09:15 # 9:15am (24-hour time is assumed)
: 09:15pm # 9:15pm
: `-S $(date -d '21 days ago' +%D-%R) -E $(date -d '17 day ago' +%D-%R)` # the date command can interpret many human-readable expressions, then express them using the format mentioned afterwards. Cool, eh!!!
: 04/15  # the bash shell will probably have problems with the forward slash unless you surround the string with double quotes

## Job Time Limit
If you want to filter for jobs with a certain time limit, use one or both of the `-k/--timelimit-min` and `-K/--timelimit-max` flags. To show only jobs with time limit between 48 and 72 hours that ran between 4 and 8 weeks ago:

`sacct -S $(date -d '8 weeks ago' +%D-%R) -E $(date -d '4 weeks ago' +%D-%R) -k 48:00 -K 72:00`

If you know the exact time limit of the jobs you are looking for, set both min and max time limit to the same value. For jobs with a time limit of 30 minutes that ran in the last month:

`sacct -S $(date -d 'last month' +%D-%R) --timelimit-min 30 --timelimit-max 30`

## **Job State Values**
Using the `-s <states>` option, you can prune your search by looking for only jobs which match the state you need, such as F for failed. (All of these work: f, failed, F, FAILED). You can specify more than one state if you separate them with commas. Note that you use a different flag for state with the `squeue` command, `-t <states>`.

Job states have short names consisting of one or two letters, and a full name.  You can use either form when working with SLURM commands. They are shown here capitalized for emphasis but can be specified as lower-case.

!!! Warning
    Different steps of a job can have different end states. For example the "extern" step is often COMPLETED when the "batch" and overall steps are FAILED. The `-X` flag to `sacct` will show you only the overall job state, such as FAILED, which is useful for most cases. However, _sometimes you need to check the state of all of a jobs steps_ in order to see that a "batch" step ran OUT_OF_MEMORY.

Primary job states of interest:

--8<-- "slurm/includes/common-job-states.md"

??? Note "Click for a complete list of job states"
    --8<-- "slurm/includes/all-job-states.md"

That complete list comes from [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAG) of the `sacct` manual page, which has also been saved to a text file you can copy for your own reference: `/jhpce/shared/jhpce/slurm/docs/job-states.txt`

## **Output Fields of Interest**

??? Note "All sacct fields (output of sacct -e)"
    --8<-- "slurm/includes/sacct-output-fields.md"

```Shell title="What output fields are available?" linenums="0"
sacct -e
```

```Shell title="See all fields for a job" linenums="0"
sacct -o ALL -j <jobid>
```

The following fields are probably the ones you'll want. See [this section](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html#lbAF) of the manual page for the list and their meaning. Capitalization does not matter; it is used for readability.

- TRES means Trackable RESources, such as RAM and CPUs.
- A number of fields (not listed) are available to tell you on which node a maximum occurred. Similarly there are fields to tell you minimum, average and maximum values for some items.

=== "Basics"
    - User
    - JobId
    - JobName
    - Partition
    - State
    - ExitCode

=== "Times"
    - Submit
    - Start
    - Elapsed
    - End

=== "Nodes"
    - AllocNodes
    - NNodes - number of nodes
    - NodeList

=== "Resources Requested"
    - ReqTRES # this is what you will be billed for
    - ReqNodes
    - ReqCPUS

=== "Resources Consumed"
    - TRESUsageInTot
    - CPUTime - (elapsed)*(AllocCPU) in HH:MM:SS format
    - MaxRSS - Max resident set of all tasks in job
    - MaxVMSize - Max virtual memory of all tasks in job
    - MaxDiskRead - Number bytes read by all tasks in job
    - MaxDiskWrite - Number bytes written by all tasks in job

### **About Memory Fields**

**Virtual Memory Size (VMSize)** is the total memory size of a job. It includes both memory actually in RAM (the RSS) and parts of executabilities which were not needed to be read in off of disk into RAM. Because, for example, routines in dynamically linked libraries were never called, so those libraries were not loaded. 

**Resident set size (RSS)** is the portion of memory (measured in megabytes) occupied by a job that is held in main memory (RAM). The rest of the memory required by the job exists in the swap space or file system, either because some parts of the occupied memory were paged out, or because some parts of the executable were never loaded.

## **Formatting fields**

By default fields are 20 characters wide. That is often insufficient.

You can put a "%NUMBER" after a field name to specify how many characters should be printed, e.g.
   
- format=name%30 will print 30 characters of field name right justified.  
- format=name%-30 will print 30 characters left justified.

You can specify your format on the command line or define an environment variable to hold the desired string (see below). (Or you can do both, and the command line will overrule what is in the environment variable. So you can define SACCT_FORMAT to be what you normally want to see but still see what you need to using the CLI.)

## **Using Environment Variables**

You can define environment variables in your shell to reduce the complexity of issuing sacct commands. You can also set these in shell scripts so that the `sacct` commands inside the script will use those values Command line options will always override these settings. Put the one you use most often into your `~/.bashrc` file.

**SACCT_FORMAT**

**SLURM_TIME_FORMAT**

#### **Formatting Dates/Times**
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

## **Exit Error Codes**
In addition to the job's "state", SLURM also records error codes. Unfortunately the vendor's [Job Exit Codes](https://slurm.schedmd.com/job_exit_code.html) page doesn't provide a meaning for the numerical values.

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

We will try to put specific entries here that we see more frequently and can identify.

|Exitcode|Probable Meaning|
|---|----|
|0:9|cancelled (does it indicate who cancelled - user or slurm?)
|0:15|cancelled (looks like this is used when either user cancelled or job ran out of time)
|0:53|Some file or directory was not readable or writable, ran out of disk space or reached disk quota|
|0:125|Job ran out of memory|
|1:0|??|
|2:0|??|

## **Diagnostic Arguments**

These can be useful to double-check what someone actually did.

```Shell title="See the full command issued to submit the job" linenums="0"
sacct -o SubmitLine%250 -j <jobid> # may need to increase field width
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

## **Examples**

**Please try to specify what you need so you don't tax the system and also have less output to inspect.**

!!! Example "All jobs for username bob that (ran with a wall time of at least 2 days) and (were killed for running out of memory) in the past 3 months:"

    ```
    sacct --user bob --starttime $(date -d '3 months ago' +%D-%R) --state OUT_OF_MEMORY --timelimit-min 2-00:00:00 --format JobID,JobName,Elapsed,NCPUs,TotalCPU,CPUTime,ReqMem,MaxRSS,MaxDiskRead,MaxDiskWrite,State,ExitCode
    ```

!!! Example "HOW MANY JOBS FAILED WITH OUT-OF-MEMORY ERRORS IN THE LAST TWO WEEKS?""
    ```
    sacct -n -S now-14days -E now -X -s OOM | wc -l
    ```

!!! Example "HOW MANY JOBS FAILED WITH OTHER KINDS OF ERRORS IN THE LAST TWO WEEKS?""
    ```
    sacct -n -S now-14days -E now -X -s F | wc -l
    ```
    ??? Caution "If you have a lot of questions about the last 2 weeks worth of jobs..."
        If you were wanting to look at the data for a lot of jobs, do SLURM users a favor and save the information to a text file. Then use normal UNIX commands like `grep` to work with the text file contents. Rather than keep asking the SLURM accounting database for variations of the information. Because doing that interrupts the master SLURM demon which manages **everything**.

!!! Example "CPU & RAM expended by jobs dying from out-of-memory errors in last 2 hours"
    ```
    sacct -n -S now-2hours -E now -u jzhou1 -X -s OOM -o cputimeraw,reqmem --units=G |  awk '{timesum+=$1;ramsum+=$2} END{printf "%s (CPU hrs) \t%s (GB)\n",timesum/60, ramsum}'
    ```
    
!!! Example "Some stats don't show up if you use the -X flag!!!!"

    Here we look at jobs in the SAS partition for the last day for a specific user. The job summary line, which is all that you would see if you used the `-X` flag, does NOT include the memory information. (You also don't see that for the RUNNING job--you'd need to use the `sstat` command for that info.) And lastly we note that the state of each job step can differ.
    
    ```
    sacct -ar sas -o user%18,jobid,state,nodelist,maxrss,maxvm,reqmem -S now-1days -E now -u c-egutie16-55548 --units=G
    ```
    ??? Note "Click here to see output"
        ```
                         User JobID             State        NodeList     MaxRSS  MaxVMSize     ReqMem 
        ------------------ ------------ ---------- --------------- ---------- ---------- ---------- 
          c-egutie16-55548 11546            FAILED     compute-132                               5G 
                       11546.extern  COMPLETED     compute-132          0          0            
                       11546.0          FAILED     compute-132      1.88G      5.64G            
          c-egutie16-55548 11565            FAILED     compute-132                               5G 
                       11565.extern  COMPLETED     compute-132          0          0            
                       11565.0          FAILED     compute-132      1.86G      5.63G            
          c-egutie16-55548 11569         COMPLETED     compute-132                               5G 
                       11569.extern  COMPLETED     compute-132          0          0            
                       11569.0       COMPLETED     compute-132      1.22G      4.98G            
          c-egutie16-55548 11589        OUT_OF_ME+     compute-132                              15G 
                       11589.extern  COMPLETED     compute-132          0          0            
                       11589.0      OUT_OF_ME+     compute-132     16.49G     16.94G            
          c-egutie16-55548 11600           RUNNING     compute-132                             400G 
                       11600.extern    RUNNING     compute-132                                  
                       11600.0         RUNNING     compute-132
        ```
!!! Example "sstat"
    Here we see the stats **at this moment** for a running job. You can embed multiple `sstat` commands in your batch job files to check on various values over the life of your job. Unfortunately `sstat` doesn't have a `--units` argument. So here we see memory in kilobytes and output writing in bytes.
    ```
    sstat -j 11600 -o maxrss,maxvmsize,maxdiskwrite
    MaxRSS  MaxVMSize MaxDiskWrite 
    ---------- ---------- ------------ 
    61259220K 203077796K  55875510365
    ```
!!! Example "Using environment variables to control output format"
    ```Shell title="Day of week MM-DD HH:MM" linenums="0"
    export SLURM_TIME_FORMAT="%a %m-%d %H:%M" 
    ```

    The start and end field widths show below are suitable for the time format shown above.

    ```Shell title="Resources requested, used" linenums="0"
    export SACCT_FORMAT="user,jobid,jobname,nodelist%12,start%-20,end%-20,state%20,reqtres%40,TRESUsageInTot%200"
    ```
