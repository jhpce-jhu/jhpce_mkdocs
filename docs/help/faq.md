---
tags:
  - needs-review
  - in-progress
---
# FAQ

There is a dedicated [SLURM FAQ](../slurm/slurm-faq.md) document.

## Why does bash report that it can’t find the module command?

??? "Click to expand answer"
    If you receive a message like
    
    ```bash linenums="0"
    bash: module: command not found
    ```
    
    The module is a shell function that is declared in `/etc/bashrc`. It is always a good idea for `/etc/bashrc` to be sourced immediately in you `~/.bashrc`.  Edit your `.bashrc` file so that the first thing it does is o execute the system bashrc file, i.e. your `.bashrc` file should start with the following lines:

    ```bash linenums="0"
    if [ -f /etc/bashrc ]; then
    . /etc/bashrc
    fi
    ```

## My script is giving odd error messages about `\r` or `^M`.

??? "Click to expand answer"
    Windows and Unix use different characters to indicate a new line.  If you have uploaded your script from a Windows machine, it may have the Windows newline characters.  These need to be replaced by the Unix newline characters.  To do this, you can run the “dos2unix” command on your script `dos2unix myscript.sh`. This will strip out all of the Windows newlines and replace them with the Unix newlines.

## What is the SAFE desktop?

??? "Click to expand answer"
    The SAFE desktop is a virtual Windows computer that you can use to run scientific software and access JHPCE. For more information see this [item](../access/access-overview.md#safe-desktop).

## I’m getting X11 errors when using rstudio with Putty and Xming

!!! Obsolete
    As of 20240220 vcxsrv is not installed on the cluster. This FAQ item needs to be reviewed and probably removed. JRT
    
We’ve had issues reported by users of Putty with Xming. One solution
we’ve found is to use the (vcxsrv))[https://sourceforge.net/projects/vcxsrv/] instead Xming newlines.  (This commmand is on all nodes.)


## How can I add packages into emacs on the cluster?

Make sure to `module load emacs` to get a later version of emacs. Then one needs to edit their `.emacs` file in their home director to include the package repos
    ```emacs
    (require 'use-package)
    (add-to-list 'package-archives '("gnu" . "http://elpa.gnu.org/packages/") t)
    (add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)
    ```
    After restaring emacs then one can do `M-x package-list-packages` and follow the GUI.


## Xauth error messages from MacOS Sierra when using X11 forwarding in SSH

With the upgrade to MacOS Sierra, the “-X” option to ssh to enable X11
forwarding may not work.  If you receive the message: `untrusted X11
forwarding setup failed: xauth key data not generated` , you can
resolve the issue by add the line `ForwardX11Trusted yes` to your
`~/.ssh/config` file on your Mac. You may still see the warning:
Warning: `No xauth data; using fake authentication data for X11
forwarding.` To eliminate this warning, add the line `XAuthLocation
/usr/X11/bin/xauth` to your `~/.ssh/config` file on your Mac.

## When running SAS, an error dialog pops up about Remote Browser

When running SAS, you may need to specify options to indicate which
browser to use when displaying either help or graphical output. We recommend
using the Chromium browser.

See our [SAS usage document](../sw/sas.md) about how to resolve this issue.


## I’m on a Mac, and the `~C` command to interrupt an ssh session isn’t working
  
It used to, but I upgraded MacOS and now it does not work.*  Newer
versions of MacOS have disabled by default the ability to send an SSH
Escape with `~C` (++tilde++ ++shift+c++).  To reenable this, on you Mac, you need to set the
`EnableEscapeCommandline` option.  You can do this by either running
`ssh -o EnableEscapeCommandline=yes . . .` or by editing your
`~/.ssh/config` file, and at the top of that file add the line:

```
EnableEscapeCommandline=yes
```

## How do I get the Rstudio program to work on the cluster?

See our [core R support document](../sw/r-n-friends.md) about this and other R usage.


## My X11 forwarding stops working after 20 minutes 

This error comes from the `ForwardX11Timeout` variable, which is set
by default to 20 minutes.  To avoid this issue, a larger timeout can
be supplied on the command line to, say, 336 hours (2 weeks):

```
$ ssh -X username@jhpce01.jhsph.edu -o ForwardX11Timeout=336h
```

## How do I copy a large directory structure from one place to another.

Please do not copy or move anything except a small set of files on the login nodes.

As an example, to copy a directory tree from `/home/bst/bob/src` to
`/dcs07/bob/dst`, first, create a cluster script, let’s call it
`copy-job`, that contains the line `rsync -avzh /home/bst/bob/src/
/dcs07/bob/dst/`. Next, submit this script as a batch job to the
cluster. An example SLURM batch job can be found [here](../slurm/crafting-jobs.md/#copying-data-within-cluster).

## My app is complaining that it can’t find a shared library, e.g. `libgfortran.so.1` 

Nine times out of ten, the allegedly missing library is there. The
problem is that your application is looking for the version of the
library that is compatible with the old system software. It will not
help to point your application to the new libraries. They are more
than likely to be incompatible with the new system. The correct
solution is to reinstall your software. If the problem persists after
the reinstallation, then please contact us and we will install
standard libraries that are actually missing.

## ssh gave a scary warning: `REMOTE HOST IDENTIFICATION HAS CHANGED`

Go into the `~/.ssh` directory of your laptop/desktop and edit the
known_hosts file.  Search for the line that starts with the host that
you ssh’d to. Delete that line (it is probably a long line that
wraps). Then try again

## Why aren’t SLURM commands, or R, or matlab, or… available to my cron job?

!!! Warning "Authoring note"
    This info needs to be re-written for upgraded cluster. And moved to the [SLURM FAQ](../slurm/slurm-faq.md). Should the `scrontab` command be mentioned?

`cron` jobs are not launched from a login shell, but the module
commands and the JHPCE default environment is initialized
automatically only when you log in. Consequently, in a cron job, you
have to do the initialization yourself. Do this by wrapping your cron
job in a bash script that initializes the module command and then
loads the default modules. You bash shell script should start with the
following lines:

```Shell
#!/bin/bash

# Source the global bashrc
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi
module load JHPCE_DEFAULT_ENV
```
## I've deleted files but my quota hasn't changed

See [this document](../storage/quotas.md/#file-deletion-and-delayed-change-in-quota).
