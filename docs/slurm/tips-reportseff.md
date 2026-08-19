---
icon: material/eye
tags:
  - slurm
---
# **reportseff useful command examples**

!!! Caution 
    If you have not already read about basic job facts like job notation, steps, names and states, please first visit the "About SLURM Jobs" document [here](../slurm/about-jobs.md).

IT support people at Princeton University created [the jobstats suite of tools](https://princetonuniversity.github.io/jobstats/) to help manage cluster resources. `seff` and `reportseff` are small parts of that suite. They are extremely useful when inspecting the behaviors of your jobs.

## **The seff (Slurm EFFiciency) command**

`seff` is a program which looks up accounting data using the `sacct` command to show some stats for a ***single*** _completed_ job.

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
## **Reportseff Overview**

`Reportseff` is a python script which makes displaying job efficiency possible (and easy!) when looking at many jobs, such as: "all of a user's jobs for a time period" or "all of the elements of an array job". The Princeton [reportseff page](https://princetonuniversity.github.io/jobstats/tools/reportseff/) describes the big picture well.

It uses `sacct` to retrieve information. It was written to accept "sacct-like" arguments and syntax. It can also pass some arguments straight through to `sacct`. This can be a bit confusing until you get used to it. For example, when indicating time ranges, it uses it's own ``--since` instead of `sacct's` `--starttime` or `-S`. But the way you express time matches the way `sacct` works, e.g. `--since now-4days`.  `reportseff` has a default output format, but you can pass along additional field names from the list that `sacct` accepts (you can see a field list with `sacct -e`).

You can create data files with `sacct` and process them with `reportseff`

Home page for more information: [https://github.com/troycomi/reportseff](https://github.com/troycomi/reportseff)

!!! Example "Example reportseff output"
    ``` title="Show my COMPLETED jobs between noon and now, adding 2 fields"
    module load reportseff
    export LESS="-R"
    
    reportseff  --since now-3days --until now -s CD --format +reqcpus,reqmem
      JobID    State       Elapsed  TimeEff   CPUEff   MemEff   ReqCPUS   ReqMem 
      12394  COMPLETED    00:44:58   0.2%     92.3%    113.0%     50      1600G
      12403  COMPLETED    00:45:12   0.2%     91.2%    115.2%     50      1600G
      12413  COMPLETED    00:00:04   0.0%      ---      0.0%      70      1800G
      12415  COMPLETED    00:00:03   0.0%      ---      0.0%      70      1800G
      12416  COMPLETED    00:00:03   0.0%      ---      0.0%      75      1800G
      12418  COMPLETED    00:03:04   0.0%      1.8%     0.9%      50      1500G
      12419  COMPLETED    00:03:24   0.0%      ---      0.0%      50      1500G
      12435  COMPLETED    00:07:03   0.0%     49.1%    46.5%      20       500G
      12455  COMPLETED    06:22:15   26.5%     9.6%    38.1%       1       50G
      12464  COMPLETED    00:01:41   0.0%     16.6%     3.2%      15       350G
      12468  COMPLETED    00:02:56   0.0%     48.1%    54.0%      10       250G
      12469  COMPLETED    08:39:19   1.8%     96.3%    97.7%      10       400G
      12503  COMPLETED    06:10:40   1.3%     91.7%    152.1%     10       380G
      12531  COMPLETED    03:24:26   0.7%     86.8%    159.9%     10       350G
      12539  COMPLETED    00:37:51   0.1%      5.1%    46.8%      20       100G
    ```

??? Example "Second example reportseff output"
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

So I was able to run `reportseff -u someuser --since now-4days`

I suppose that they chose their argument flags to avoid overlapping with those used by sacct, so you see things like `--since` and `--until` instead of `sacct`'s `-S` and `-E`  But most `sacct` arguments will work. 

Note that you can add `sacct` fields to `reportseff`'s default ones with a simple `--format +fieldname,otherfield`

### GPU SUPPORT?
GPU jobs consume not only system CPU and RAM but also, of course, resources on GPU cards. IT support people at Princeton University created [the jobstats suite of tools](https://princetonuniversity.github.io/jobstats/) to help manage cluster resources. `seff` and `reportseff` are small parts of that suite.

They have the ability to calculate efficiency figures for GPU jobs. Users and administrators alike would love to see that info. However, a bunch of work is required to install and configure the tools which reach into each node on a frequent basis to gather information and tie it to specific SLURM jobs!!! 
(`sacct` has no native ability to interrogate GPU tools like `nvidia-smi`. `nvidia-smi` has not concept of SLURM jobs.) We probably won't have time to do this work. It doesn't help that we have a wide diversity of GPU models. 

### COLORIZED OUTPUT
It seems to use `less` as its pagination tool by default when output is longer than your terminal's size. Normally it produces colorized output which is *_very nice_*, as Borat would say. But the colors go away and you see the ugly escape characters unless you have the LESS environment variable defined to include `-R`. So you may want to add to your `~/.bashrc` file a command like `export LESS="-i -M -R"` (The `-i` means "do case-insensitive searches, unless a capital is used", `-M` means "make prompt show lines and current byte" and `-R` means "show color text where ESC sequences are present".) 

Adding the flag `--color` did not help. There is a `--no-color` which might be handy for generating reports lacking escape characters when redirecting the output to a file.

### MEMORY USE ABOVE 100%
Like other tools I've used which provide memory efficiency stats, both `seff` and `reportseff` commonly shows jobs where memory efficiency is above 100%.  However many jobs with memory use above 100% run through to completion, indicating that they did not, in fact, exceed the RAM they requested. SLURM uses UNIX "cgroups" to control jobs access to CPU cores and RAM.

Possible explanations:

1. **What is "memory" anyway?** - A computer program's memory in RAM is divided into specific segments when running. Aside from the *stack* (which handles function calls and local variables), key memory components include the *heap* for dynamic data, the *text* segment for executable code, and *data segments* for global variables. If you look at available `sacct` output fields (using `sacct -e`) you will see a variety of memory-related ones.
2. **Challenges of adding up memory** - Modern programs make use of many software libraries even if they don't call them directly and explicitly. UNIX has been tuned over decades to be as efficient as possible, so it will only read into RAM the parts of a library that it needs at the time. {==My primary suspect for memory totals over 100% is that `sacct` and therefore `reportseff` are adding together the whole sizes of linked libraries and reporting that, rather than the maximum amount actually ever loaded into RAM.==}
3. **Grabbing before being smacked** - There is a possibility that the job's processes grab more memory than they are allowed _before_ the operating system has time to prevent that by imposing cgroup limits. Countering this idea is the large number of times jobs complete successfully.
4. One explanation formemory numbers that are "off" that I've seen is that the `reportseff` code is using MaxVM instead of MaxRSS etc. I happen to see in one of the parts of `reportseff` (the file`output_renderer.py`) a line `"MemEff": ["REQMEM", "NNodes", "AllocCPUS", "MaxRSS", "NTasks"]` One would need to pore over the code to ensure the "correct" fields are being used in every place.
