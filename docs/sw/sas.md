# SAS

## We Have A SAS Partition

We created a "sas" partition because we have a limited number of licenses. The command `squeue --partition=sas` will display running and pending SAS jobs. Please use this partition by specifying `-p sas` when submitting your jobs. You can see the available resources in this partition with the command `slurmpic -p sas` If your job will not fit in the "sas" partition or you want to run your job in a PI partition you are authorized to use, you can use a different one.

The amount of memory you request for your interactive session depends on the size of the data you will be manipulating! SAS has extensive internal support for dividing up work across threads and cores, so also consider requesting more CPU cores for large datasets and complex analyses. If you don't specify those resources, your job will run with 1 core and 5GB of RAM. The job will die if it exceeds the RAM requested.

## Other ways to access SAS
SAS is available in a free virtual Windows environment called the "SAFE Desktop" maintained by Hopkins central IT.

If you need another place to run SAS, to for example learn how to use the application, you might want to register for this free service. (That can take a business day or two to fulfill.) 

[Here is a link](../access/safe-safer-desktops.md) to more information about SAFE, including acquiring a SAFE account.

## MarketScan Database

Many of our SAS users work with this licensed database, which is provided by Caleb Alexander & Jodi Segal. {==These files must not be copied==} to other locations without consulting the co-PIs and the licensing agreement.

### How To Get Permission To Access It
Contact  Shanshan Lin  <slin95@jhu.edu> in the Epidemiology Department.
Her group controls the access to the Marketscan data on the JHPCE cluster.  They have some
good guides on how the data is organized on the cluster.  She will then ask us to add you to
the "marketscan" UNIX groups so you can see the database's files.

### Where Is It Located?

As of October 2026 it is located in several subdirectories inside `/dcs10/alexander/data/`

## Using SAS interactively

Interactive use of the graphic SAS interface can be slow, as the X11 windowing system software which provides the graphics can be slow when so many network segments lie between you and the compute node in our cluster.

SAS jobs can be run in batch mode, which both avoids the slow networking and allows you to queue up a series of jobs to run without being present. See our [SAS batch mode](sas.md#running-sas-job-in-batch-mode) section below.

### Without Browser Support
If you do not intend to do any *plotting* or use the *built-in help*, you can start sas with a simple "sas" command.
```
[test@jhpce03 ~]$ srun --partition sas --mem 20G --cpus-per-task=4 --x11 --pty bash
srun: job 3178287 queued and waiting for resources
srun: job 3178287 has been allocated resources

[test@compute-101 ~]$ module load sas
[test@compute-101 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) sas/9.4

[test@compute-101 ~]$ sas
```

### With Browser Support

To display either *built-in help* or *graphical output* when running SAS, you need to specify options to allow a web browser to be launched *if called for* (it does not run otherwise). The `chromium-browser` is the only one installed in the cluster, so use that unless you install one of your own in your home directory.

The initial steps are the same as we used before:
```
[test@jhpce03 ~]$ srun --partition sas --mem 20G --cpus-per-task=4 --pty bash
srun: job 3178287 queued and waiting for resources
srun: job 3178287 has been allocated resources

[test@compute-101 ~]$ module load sas
[test@compute-101 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) sas/9.4
```

Our examples here do not include the arguments you need to give to SAS which specify your program file or other optional arguments.

There are two ways of adding the browser support: one which displays in your terminal any and all warnings and errors that are printed by both SAS and the browser (if it launches), and one which redirects such messages into a black hole.

!!! Note "Note"  
    - You may need to accept popups in the chromium browser that gets started in order to see the windows that SAS is trying to display.
    - If you do not redirect standard output and standard error data streams, you will see a bunch of error messages. These can be ignored because the browser wants to run on a local system with a graphics card, and the our compute nodes don't have such cards or the software to support them. HOWEVER, redirecting all I/O means that if SAS itself emits important error messages, you won't see them. So if SAS isn't working as expected, you need to remember to not redirect I/O.

Choose one of the following variations. These are only for use in interactive sessions. Do not use them in batch jobs.

Note that you can click on the faint "pages" icon at the right end of the section to copy the contents to your copy-and-paste buffer):

``` title="Chrome, no I/O redirection"
sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'"
```

``` title="Chrome, all I/O redirected to a black hole"
sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'" "$@" > /dev/null 2>&1
```

??? tip "Define an alias for those commands for a better life"
    Obviously those sas-with-browser-support commands are painful to have to enter by hand. You can add to your `~/.bashrc` file a bash function which contain these complex commands.  Once that becomes part of your active environment (by sourcing the file (`. ~/.bashrc`) if you just changed that file, or by logging out and back in again), you can start SAS using a simple `csas`. This function include the `$@` variable, which means "insert here any additional arguments the user provided".  Therefore you can run "csas -o some_argument my_sas_program.sas". Note that you can click on the faint pages icon at the right end of the section to copy the contents to your copy-and-paste buffer.

    !!! Warning
    These definitions include optional syntax (`> /dev/null 2>&1`) which hide error messages. If you are having problems launching or using SAS, you may need to run SAS without that output redirection!!!! The ["srun half duplex"](../slurm/slurm-faq.md/#srun-error-_half_duplex) error is an example of such a case.

    ```Shell
    # SAS routines for __interactive__ sessions where plotting/help is involved
    # (because SAS generates HTML for the plots when run in interactive mode)
    # 
    # (YOU HAVE TO RUN "module load SAS" before calling either of these routines. Or
    #  you could include them into the function definitions)
    #
    csas() { sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'" "$@" > /dev/null 2>&1 }
    # This one does not do I/O redirection:
    csasfull() { sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'" "$@" }
    ```

## Running SAS job in batch mode

As mentioned earlier, you may need to request more RAM and/or CPU cores depending on the work you do. 

1. Write your sas source in a file with .sas extension (e.g. class-info.sas)
```SAS
DATA CLASS;
     INPUT NAME $ 1-8 SEX $ 10 AGE 12-13 HEIGHT 15-16 WEIGHT 18-22;
CARDS;
JOHN     M 12 59 99.5
JAMES    M 12 57 83.0
ALFRED   M 14 69 112.5
ALICE    F 13 56 84.0

PROC MEANS;
     VAR AGE HEIGHT WEIGHT;
PROC PLOT;
     PLOT WEIGHT*HEIGHT;
ENDSAS;
;
```

2. Write a submit job script (e.g. sas-demo1.sh)
```bash
#!/bin/bash

#SBATCH --partition=sas
#SBATCH --mem=2G
#SBATCH --time=2:00

module load sas
sas class-info.sas
```

3. Submit your matlab job
```
[test@login31 ~]$ sbatch sas-demo1.sh
Submitted batch job 3178475
```

4. Monitor the job status
```bash
[test@login31 ~]$ squeue --me
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            3178475       sas sas-demo    test R       0:01      1 compute-101
```

5. When the job is finished, the output files are created in your Current Working Directory
```
-rw-r--r-- 1 test test    0 Mar 13 16:49 slurm-3178475.out
-rw-r--r-- 1 test test 2108 Mar 13 16:49 class-info.lst
```

6. Look at the results from output file
```
[test@login31 ~]$ cat class-info.lst

                                                        The MEANS Procedure

                           Variable    N            Mean         Std Dev         Minimum         Maximum
                           -----------------------------------------------------------------------------
                           AGE         4      12.7500000       0.9574271      12.0000000      14.0000000
                           HEIGHT      4      60.2500000       5.9651767      56.0000000      69.0000000
                           WEIGHT      4      94.7500000      14.0386372      83.0000000     112.5000000
                           -----------------------------------------------------------------------------

```

## Tips for Using The SAS Editor

Duke's Sociology department has a page titled [Giving the SAS Program Editor Under Linux A Windows Look and Feel](https://computing.soc.duke.edu/computing/Giving(20)the(20)SAS(20)Program(20)Editor(20)Under(20)Linux(20)a(20)Windows(20)Look(20)and(20)Feel.html)

Other advice can be found in [this presentation](https://www.lexjansen.com/phuse/2015/is/iS07_ppt.pdf) for Windows users transitioning to using SAS in a Linux environment.
bac
