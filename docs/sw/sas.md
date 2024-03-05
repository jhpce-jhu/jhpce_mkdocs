---
tags:
  - in-progress
  - jiong
---

# SAS

## We Have A SAS Partition

Containing two nodes.

## Other ways to access SAS
SAS is available in a virtual Windows environment called SAFE. [Here is a link](../access/access-overview.md#safe-desktop) to information about SAFE.

## MarketScan Database

### How To Get Permission To Access It
### Where Is It

## When running SAS, an error dialog pops up about Remote Browser

When running SAS, you may need to specify options to indicate which
browser to use when displaying either help or graphical output. We recommend
using the Chromium browser. You can use the following options to the
SAS command to do so:

`sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'"`

Here is some code you can add to your .bashrc file which contain some convenient bash aliases for starting SAS with browser support configured. Once that becomes part of your environment (by sourcing the file or by logging out and back in again), after loading the SAS module you can start SAS using either `csas` or `fsas` so that it can open the desired web browser if needed.

!!! Warning
    These definitions include optional syntax (`> /dev/null 2>&1`) which hide error messages. If you are having problems displaying SAS material in web browsers, you may need to run SAS without that output redirection. The ["srun half duplex"](../slurm/slurm-faq/#srun-error-_half_duplex) error is an example of such a case.

```Shell
# SAS routines for __interactive__ sessions where plotting is involved
# (because SAS generates HTML for the plots when run in interactive mode)
# 
# (YOU HAVE TO RUN "module load SAS" before calling either of these routines)
#
# If you want to use Firefox as your web browser for SAS:
#
fsas() { sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/firefox'" -xrm "SAS.helpBrowser:'/usr/bin/firefox'" "$@" > /dev/null 2>&1; }
#
# If you want to use Chromium-browser as your web browser for SAS:
#
csas() { sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'" "$@" > /dev/null 2>&1; }
```
