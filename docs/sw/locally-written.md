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
* **auth_util**: Interface for working with [OTP MFA](../access/access-overview.md/#multi-factor-authentication-mfa) configuration. View and generate verification tokens for Google Authenticator. See [orientation documents](https://docs.google.com/presentation/d/1elMSTUdKws7FLVFK7vVV_AErA4brNSPX/pub){:target="_blank"} for details on using it to configure your smartphone, etc. (no man page yet)

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

We are creating a growing number of scripts and resources. They are found in [this document](../slurm/slurm-commands-ref.md/#locally-written-tools).

