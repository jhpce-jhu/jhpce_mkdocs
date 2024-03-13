## **Running basic SAS jobs over SLURM**

### - Using SAS interactively

```
[test@login31 ~]$ srun --partition sas --mem 10G --x11 --pty bash
srun: job 3178287 queued and waiting for resources
srun: job 3178287 has been allocated resources

[test@compute-101 ~]$ module load sas
[test@compute-101 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) sas/9.4

[test@compute-101 ~]$ sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'"

## if you want to use Firefox as your web browser for SAS, run the following command instead
[test@compute-101 ~]$ sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/firefox'" -xrm "SAS.helpBrowser:'/usr/bin/firefox'"
```

Notes:  
  * You may need to accept popups in the chromium/firefox browser that gets started in order to see the windows that SAS is trying to display  
  * In the terminal session that you started “sas”, you may see messages similar to ones below.  These can be ignored because the browser wants to run on a local system with a graphics card, and the X11 session doesn’t allow that
```
[2970887:2970887:1024/152818.092311:ERROR:chrome_browser_cloud_management_controller.cc(163)] Cloud management controller initialization aborted as CBCM is not enabled.
[2970887:2971086:1024/152818.161869:ERROR:login_database.cc(922)] Password store database is too new, kCurrentVersionNumber=35, GetCompatibleVersionNumber=39
[2970887:2971087:1024/152818.164247:ERROR:login_database.cc(922)] Password store database is too new, kCurrentVersionNumber=35, GetCompatibleVersionNumber=39
[2970887:2971086:1024/152818.167534:ERROR:login_database_async_helper.cc(59)] Could not create/open login database.
[2970887:2971087:1024/152818.170351:ERROR:login_database_async_helper.cc(59)] Could not create/open login database.
[2970887:2970887:1024/152818.626429:ERROR:object_proxy.cc(590)] Failed to call method: org.freedesktop.portal.Settings.Read: object_path= /org/freedesktop/portal/desktop: org.freedesktop.portal.Error.NotFound: Requested setting not found
libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast
```

### - Using SAS in batch mode

1. Write your sas source in a file with .sas extension (e.g. class-info.sas)
```
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
```
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
```
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
