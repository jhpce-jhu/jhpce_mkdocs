---
icon: material/eye
tags:
  - in-progress
  - slurm
---
# **reportseff useful command examples**
 
!!! Caution 
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

you can define environment variables as you do with sacct to change your formatting. See [here](../slurm/tips-sacct.md/#using-environment-variables)

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
