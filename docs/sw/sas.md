# SAS

## We Have A SAS Partition

We have a limited number of licenses. They run on computers with limited amounts of RAM. The command `slurmpic -p sas` will display the current resources used out of total available.

Please use this partition by specifying `-p sas` when submitting your jobs. You can see the available resources in this partition with the command `slurmpic -p sas` If your job will not fit in the partition or you want to run your job in a PI partition you are authorized to use, you can use a different one.

## Other ways to access SAS
SAS is available in a virtual Windows environment called SAFE maintained by Hopkins central IT. For small jobs,
or collaborating on files with private personal information with others, or if you don't have SAS installed on your
personal computer, you might want to register for this free service (you have to wait for someone to create your account).

[Here is a link](../access/access-overview.md#safe-desktop) to information about SAFE.

## MarketScan Database

Many of our SAS users work with this licensed database.

### How To Get Permission To Access It
Contact  Xuya (Rayna) Xiao  <xxiao14@jhmi.edu> in the Epidemiology Department.
Her group controls the access to the Marketscan data on the JHPCE cluster.  They have some
good guides on how the data is organized on the cluster.  She will then ask us to add you to
the "alexander" UNIX group so you can see the database's files.

### Where Is It Located?

As of April 2024 it is located in `/dcl02/alexande/data/MARKETSCAN/`

As of May 2025 the latest version of the data is located in `/dcl02/alexande/data/MARKETSCAN2025/data`

## Using SAS interactively

The amount of memory you request for your interactive session depends on the size of the data you will be manipulating.

If you learn how to work with SAS in batch mode, you can execute jobs without having to wait for slow X11 protocol transmissions of the SAS graphic interface. And you can queue up a series of jobs to run without being present.  See our [SAS batch mode](./sas.md#running-sas-job-in-batch-mode) section below.

### Without Browser Support
If you do not intend to do any *plotting* or use the *built-in help*, you can start sas with a simple "sas" command.
```
[test@login31 ~]$ srun --partition sas --mem 10G --x11 --pty bash
srun: job 3178287 queued and waiting for resources
srun: job 3178287 has been allocated resources

[test@compute-101 ~]$ module load sas
[test@compute-101 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) sas/9.4

[test@compute-101 ~]$ sas
```

### With Browser Support

To display either *built-in help* or *graphical output* when running SAS, you need to specify options to allow a web browser to be launched if called for. We recommend using the Chromium browser

!!! Note "Note"  
    - You may need to accept popups in the chromium browser that gets started in order to see the windows that SAS is trying to display.
    - If you do not redirect standard output and standard error data streams, you will see a bunch of error messages. These can be ignored because the browser wants to run on a local system with a graphics card, and the our compute nodes don't have such cards or the software to support them. HOWEVER, redirecting all I/O means that if SAS itself emits important error messages, you won't see them. So don't redirect I/O if SAS isn't working as expected!

The initial steps are the same as we used before:
```
[test@login31 ~]$ srun --partition sas --mem 10G --x11 --pty bash
srun: job 3178287 queued and waiting for resources
srun: job 3178287 has been allocated resources

[test@compute-101 ~]$ module load sas
[test@compute-101 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) sas/9.4
```

Your next step is choose which SAS command to run and
which optional arguements to give it to about your files or how you want SAS to behave.

Choose one of the following variations (note that you can click on the faint pages icon at the right end of the section to copy the contents to your copy-and-paste buffer):

``` title="Chrome, no I/O redirection"
sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'"
```


``` title="Chrome, all I/O redirected to a black hole (see tip below)"
csas
```


??? tip "Define these handy aliases for a better life"
    Here is some code you can add to your .bashrc file which contain some convenient bash aliases for starting SAS with browser support configured. Once that becomes part of your environment (by sourcing the file (`. ~/.bashrc`) or by logging out and back in again), after loading the SAS module you can start SAS using either `csas` or `fsas` so that it can open the desired web browser if needed. These definitions include $@ symbols, which are replaced by any additional arguments to SAS that you provide. Note that you can click on the faint pages icon at the right end of the section to copy the contents to your copy-and-paste buffer.

    !!! Warning
    These definitions include optional syntax (`> /dev/null 2>&1`) which hide error messages. If you are having problems launching or using SAS, you may need to run SAS without that output redirection!!!! The ["srun half duplex"](../slurm/slurm-faq.md/#srun-error-_half_duplex) error is an example of such a case.

    ```Shell
    # SAS routines for __interactive__ sessions where plotting is involved
    # (because SAS generates HTML for the plots when run in interactive mode)
    # 
    # (YOU HAVE TO RUN "module load SAS" before calling either of these routines)
    #
    # To use Chromium-browser as your web browser for SAS to display html help files and output:
    #
    csas() { sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'" "$@" > /dev/null 2>&1; }
    ```

## Running SAS job in batch mode

The amount of memory you request for your batch session depends on the size of the data you will be manipulating.

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
