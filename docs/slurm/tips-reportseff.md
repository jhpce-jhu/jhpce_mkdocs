---
icon: material/eye
tags:
  - in-progress
  - slurm
---
# **reportseff useful command examples**
 
!!! Warning
    This file is under heavy construction. It was created by copying an existing page for sacct. Duplicate material will be removed before it is finished.
    
    If you have not already read about basic job facts like job notation, steps, names and states, please first visit the "About SLURM Jobs" document [here](../slurm/about-jobs.md).
 
## **Reportseff Overview**

`seff` is a program which looks up accounting data using the `sacct command to show some stats for a ***single*** completed job.

??? Example "Example seff output"
    ```
    Job ID: 9709
    Cluster: cms
    User/Group: ttotoj/users
    State: COMPLETED (exit code 0)
    Cores: 1
    CPU Utilized: 00:47:34
    CPU Efficiency: 97.74% of 00:48:40 core-walltime
    Job Wall-clock time: 00:48:40
    Memory Utilized: 67.40 GB
    Memory Efficiency: 22.47% of 300.00 GB
    ```

`Reportseff` is a python script which makes displaying job efficiency easier for other cases such as "all of a user's jobs for a time period" or "all of the elements of an array job". It also uses `sacct` to retrieve information.  It was written so that you could use the same kinds of syntax as you would with `sacct`. For example when indicating time ranges or asking for changes to the output format. You can also pass other arguments straight to `sacct` if desired.

Home page for more information: https://github.com/troycomi/reportseff

!!! Example "Example reportseff output"
    ``` title="Show my COMPLETED jobs between noon and now, adding a field"
    module load reportseff
    export LESS="-R"
    
    reportseff --since noon --until now -s CD --format +user
    
        JobID    State         Elapsed  TimeEff   CPUEff   MemEff     User   
       5011394  COMPLETED      19:16:18   40.1%    99.5%    130.4%    mnagle
       5013943  COMPLETED      14:27:08   30.1%    99.9%    127.9%    mnagle
       5016834  COMPLETED      07:48:51   16.3%    98.4%    124.0%    mnagle
       5024400  COMPLETED      05:54:33   24.6%     8.0%    60.3%    enorton
       5025014  COMPLETED      04:28:52   18.7%     9.2%     3.4%    jzhang5
       5025583  COMPLETED      03:57:29   16.5%    89.4%    26.1%      sli1
       5025584  COMPLETED      04:11:32   17.5%    89.1%    26.9%      sli1
       5025586  COMPLETED      03:55:38   16.4%    82.8%    26.4%      sli1
    ```

!!! Info "There are two ways to use reportseff:"
    ??? Note "As a module:"
        ```
        module load reportseff
        export LESS="-R"  # to provide colorized output
        ```
    ??? Note "As a bash function:"
        ```
        Define an alias on the command line or in your .bashrc
        
        rs() { export LESS="-R";export PYTHONPATH=/jhpce/shared/jhpce/core/reportseff/2.3.2/;export PATH=/jhpce/shared/jhpce/core/reportseff/2.3.2/bin:$PATH; reportseff "$@"; }
        ```        


!!! Examples
    ```Shell title="Show my COMPLETED jobs between noon and now" linenums="0"
    module load reportseff
    export LESS="-R"
    reportseff --since noon --until now -s CD -u $USER
    reportseff --since noon --until now -s CD -u $USER --format "user,jobid,state,nodelist,start,end,cpueff,memeff"
    
    reportseff --since noon --until now -s CD -u $USER --format "+user,nodelist,start,end,cpueff,memeff"
    ```

you can define environment variables as you do with sacct to change your formatting. See [here](../slurm/tips-sacct/#using-environment-variables)

!!! Caution
    you may not be able to see other user's job info unless you add `--extra-args -a `

??? Example "Reportseff arguments, click for complete list"

    ```
    login31:~% reportseff --help
    Usage: reportseff [OPTIONS] [JOBS]...

    Main entry point for reportseff.

    Options:
      --modified-sort                 If set, will sort outputs by modified time
                                  of files
      --color / --no-color            Force color output. No color will use click
                                  defaults
      --format TEXT                   Comma-separated list of columns to include.
                                  Options are any valid sacct input along with
                                  CPUEff, MemEff, Energy, and TimeEff.  In
                                  systems with jobstat caching, GPU usage can
                                  be added with GPUEff, GPUMem or GPU (for
                                  both). A width and alignment may optionally
                                  be provided after "%", e.g. JobID%>15 aligns
                                  job id right with max width of 15
                                  characters. Generally
                                  NAME[[%:][ALIGNMENT][WIDTH[e$]?]]. When an
                                  `e` or `$` is present after a width
                                  argument, the output will be truncated to
                                  the right.Prefix with a + to add to the
                                  defaults. A single format token will
                                  suppress the header line. Wrap in quotes to
                                  pass a string literal, otherwise alignment
                                  may be misinterpreted.
      --slurm-format TEXT             Filename pattern passed to sbatch.  By
                                  default, will handle patterns like
                                  slurm_%j.out, %x_%j, or slurm_%A_%a.  In
                                  particular, the jobid is expected to start
                                  with '_'.  Setting this to the same entry as
                                  used in sbatch will allow parsing slurm
                                  outputs like `1234.out`.  Array jobs must
                                  have %A_%a to properly interface with sacct.
      --debug                         Print raw db query to stderr
      -u, --user TEXT                 Ignore jobs, return all jobs in last week
                                  from user
      --partition TEXT                Only include jobs with the specified
                                  partition
      --extra-args TEXT               Extra arguments to forward to sacct
      -s, --state TEXT                Only include jobs with the specified states
      -S, --not-state TEXT            Include jobs without the specified states
      --since TEXT                    Only include jobs after this time. Can be
                                  valid sacct or as a comma separated list of
                                  time deltas, e.g. d=2,h=1 means 2 days, 1
                                  hour before current time. Weeks, days,
                                  hours, and minutes can use case-insensitive
                                  abbreviations. Minutes is the minimum
                                  resolution, while weeks is the coarsest.
      --until TEXT                    Only include jobs before this time. Can be
                                  valid sacct or as a comma separated list of
                                  time deltas, e.g. d=2,h=1 means 2 days, 1
                                  hour before current time. Weeks, days,
                                  hours, and minutes can use case-insensitive
                                  abbreviations. Minutes is the minimum
                                  resolution, while weeks is the coarsest.
      -n, --node / -N, --no-node      Report node-level statistics. Adds `jobid`
                                  to format for proper display.
      -g, --node-and-gpu / -G, --no-node-gpu
                                  Report each GPU for each node. Sets `node`
                                  and adds `GPU` to format automatically.
      -p, --parsable                  Output will be '|' delmited without a '|' at
                                  the end.
      --version                       Show the version and exit.
      --help                          Show this message and exit.
    ```
  
[sacct](https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html) is a command used to display information about jobs. It has a number of subtleties, such as the time window reported on and the formatting of output. We hope that this page will help you get the information you need.

`sacct` can be used to investigate jobs' resource usage, nodes used, and exit codes. It can point to important information, such as jobs dying on a particular node but working on other nodes[^1].

[^1]: In which case you can add the directive `--exclude=compute-xxx` to your job submission, then notify us via bitsupport so we can fix that node.

sacct will show all submitted jobs but cannot, of course, provide data for a number of fields until the job has finished. Use the [sstat](https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html) command to get information about running programs. "Instrumenting" your jobs to gather information about them can include adding one or more `sstat` commands to batch jobs in multiple places.

Examples below use angle brackets ++less++ ++greater++  to indicate where you are supposed to replace argumements with your values.



### ARGUMENTS
One of the appeals of this tool is that it uses sacct of course but allows you to pass extra sacct args. And it uses the same syntax for time that sacct does. 

So I was able to run `rs -u c-yzhou172-57285 --since now-4days`

I suppose that they chose their argument flags to avoid overlapping with those used by sacct, so you see things like `--since` and `--until` instead of `sacct`'s `-S` and `-E`

### GPU support?
Another feature of interest is supposedly the ability to calculate memory use efficiency for GPU jobs. If we discovered that many users were typically not using all of the memory on GPUs, it would open the door to partitioning GPUs into slices and therefore increasing the number of available GPUs for zero dollars.

### COLORIZED OUTPUT
It seems to use less as its pagination tool by default when output is longer than your terminal's size. Normally it produces colorized output which is very nice, as Borat would say. But the colors go away and you see the ugly escape characters unless you have the LESS environment variable defined to include `-R`.  I grepped for "less" in the python files in the main reportseff subdirectory, hoping to find a place where I could just add the -R so users would benefit. 

Adding the flag `--color` did not help. There is a `--no-color` which might be handy for generating reports lacking escape characters.

### MEMORY USE ABOVE 100%
Like other tools I've used which provide memory efficiency stats, this one shows jobs where efficiency is above 100%. Which I perhaps mistakenly interpret as indicating that the user has exceeded their memory limits. (I think I've also seen this in pure sacct output where no one is trying to combine data and calculate anything.)   DO THESE CASES REPRESENT INSTANCES OF THE USER'S PROCESSES GRABBING MORE MEMORY THAN "ALLOWED" FOR BRIEF-ENOUGH PERIODS THAT CGROUP LIMITS ARE NOT RESULTING IN KILLED PROCESSES? Or some other explanation that doesn't indicate that our understandings about memory stats or cgroups are flawed?

Ultimately I would like to be able to tell users / write in the docs an explanation for why you see values above 100% even if only to aid them in understanding how to utilize the info to make their cluster use better. (At least I've never seen any negative numbers!)

One explanation for  memory numbers that are "off" that I've seen is that someone is using MaxVM instead of MaxRSS etc. I happen to see in output_renderer.py a line `"MemEff": ["REQMEM", "NNodes", "AllocCPUS", "MaxRSS", "NTasks"]` which might imply which field is being used.

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

`sacct` does not have a sort option. You need to sort its output by other methods. If you want to change the order of the information, specify the field names in the desired order.

This is a prime example of where it is kindest to the SLURM master daemon if you run your query such that you store the results in a text file, then work with the text file contents. Rather than running many variations of a command pipeline
beginning with `sacct` when you already have all of the desired output option fields and are just trying to figure out the right text-processing logic.


```shell title="sort failed jobs by exitcode"
sacct -X -a -s F -o "user,jobid,state,nodelist,exitcode" -S noon -E now|sort -k5| less
```

```shell title="sort failed jobs by exitcode, then by nodelist"
sacct -X -a -s F -o "user,jobid,state,nodelist,exitcode" -S noon -E now|sort -k5,5 -k4,4| less
```

Useful sort options:

1. `-kKEYDEF` or `--key=KEYDEF` Given just a number as the KEYDEF, it sorts by column
2. `-r`or `--reverse`
3. `-n` or `--numeric-sort` 

See the sort manual page for other options, including multiple kinds of numeric sorts, as well as the syntax of KEYDEF.

## **Start and End Times**
{==It is best to use always specify a `-S` start time and a `-E` end time.==}

Special time words: **today**, **midnight**, **noon**, **now**

Positive and negative deltas from now can be used, which is very handy, but remember to use the full unit word ("days" not "d" or "day")!!!

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

## Job Limit
If you want to filter for jobs with a certain time limit, use the `-k/--timelimit-min` and `-K/--timelimit-max` flags. To show only jobs with time limit between 48 and 72 hours that ran between 4 and 8 weeks ago:

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
    - JobiId
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

You can specify your format on the command line or define an environment variable to hold the desired string (see below).

## **Using Environment Variables**

You can define environment variables in your shell to reduce the complexity of issuing sacct commands. You can also set these in shell scripts. Command line options will always override these settings.

SACCT_FORMAT

SLURM_TIME_FORMAT

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

## **Examples**

all jobs for username bob that ran with a wall time of at least 2 days and were killed for running out of memory in the past 3 months:

`sacct --user bob --starttime $(date -d '3 months ago' +%D-%R) --state OUT_OF_MEMORY --timelimit-min 2-00:00:00 --format JobID,JobName,Elapsed,NCPUs,TotalCPU,CPUTime,ReqMem,MaxRSS,MaxDiskRead,MaxDiskWrite,State,ExitCode`
