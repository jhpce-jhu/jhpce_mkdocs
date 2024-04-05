---
tags:
  - in-progress
---

# Locally Written Tools

We here try to capture information about programs you may want to use. Most of them lack on-line manual pages. We try to add support for usage statements and the `-h` (help) argument. Because all of them are shell scripts or python, you can look at the source code.

## Where are they?

Many of these programs live in `/jhpce/shared/jhpce/core/JHPCE_tools/3.0/bin/` If that directory is not in your PATH, then you are not seeing the normal and supported JHPCE environment. (If it occurs after your additions to the PATH, then you may see only parts of the supported environment.)

Your environment, including your PATH variable, is created when logging in. A number of configuration files are processed. Your `.bashrc` file is a core one. By default it will consult one in `/etc/bashrc`  Files in `/etc/profile.d/` are also processed. It is via these latter files that we do things like provide support for [modules](../sw/modules.md), display messages of the day on login nodes, etc.

You can use the command `which cmd` to be told where "cmd" comes from.

## General
* **auth_util**: Interface for working with [OTP MFA](../access/access-overview.md/#multi-factor-authentication-mfa) configuration. View and generate verification tokens for Google Authenticator. See [orientation documents](../orient/images/latest-orient.pdf) for details on using it to configure your smartphone, etc. (no man page yet)

## Disk Usage
* **getquota**: Displays your quota stats (not yet working on C-SUB)
* **cmsquota-dcs05**: For C-SUB users, (only works on compute nodes)
* **cmsquota-dcs06**: For C-SUB users, (works on both login and compute nodes)

## Container Computing

* **jupyterlab.sh**: Start an Jupyter server available via an SSH tunnel.
* **jupyterlab-gpu.sh**: Start an Jupyter server available via an SSH tunnel with CUDA libraries in LD_LIBRARY_PATH.
* **jhpce-rstudio-server**: Start an RStudio server available via an SSH tunnel.
* **csub-rstudio-server**: Cluster-specific version.

## SLURM-related

### Information about cluster and jobs
!!! Warning
    **Do not frequently[^1] run slurmpic, squeue, sacct or other Slurm client commands using loops in shell scripts or other programs.** 

    These commands all send remote procedure calls to slurmctld, the main SLURM control and scheduling daemon, They may also perform look-ups in the accounting database. That process and the database need to be *highly responsive* to the input/output caused by running jobs.

    Ensure that programs limit calls to slurmctld to the minimum necessary for the information you are trying to gather. Add arguments to limit to needed partitions or users or job data fields, etcetera.

[^1]: Frequently meaning more than once every **five minutes**. Do you REALLY **need** to know something sooner than that? If you want to know when a job finishes, use email notification settings. You can add them to pending and running jobs using [scontrol](../slurm/tips-scontrol.md).

* **slurmpic**: ***An essential program for getting cluster status info.*** Use -h option to see essential usage details.
* **jobson**: Displays running jobs running on a node when given a three digit node number.
* **showjob**: Displays job information when given a jobid. Only works for pending or running jobs. Currently simply a shortcut for `scontrol show job jobid --details` but hopefully in the future will produce more readable output.
* **showqos**: Displays list of our [QOS definitions](../slurm/qos.md) in a readable format. (no man page yet)
* **slurmuser**: Displays per-user summary usage of RAM & CPU across the cluster. Can display by partition or for a specific user.
* **smem**: Displays memory used by your currently running jobs. If given a jobid number, it will display info about the memory usage of that job. (no man page yet)
* memory reporting script - puts per-user output daily into directories under `/jhpce/shared/jhpce/jhpce-log/`



### Submitting Jobs
None yet.

### Contributed SLURM Programs We've Installed
* **seff**: Display efficiency of CPU and RAM usage of a completed job. (no man page yet)
* **[slurm-mail:](https://github.com/neilmunday/slurm-mail)** Tool used to add details to mail sent to you. Not something you can modify. Listed for completeness.

