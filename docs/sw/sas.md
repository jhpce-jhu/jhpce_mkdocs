---
tags:
  - in-progress
---

# SAS

## We Have A SAS Partition

Containing two nodes.

## MarketScan Database

### How To Get Permission To Access It
### Where Is It

## When running SAS, an error dialog pops up about Remote Browser

When running SAS, you may need to specify options to indicate which
browser to use when displaying either help or graphical output. We recommend
using the chromium browse, and you can use the following options to the
sas command to do so:

`sas -helpbrowser SAS -xrm "SAS.webBrowser:'/usr/bin/chromium-browser'" -xrm "SAS.helpBrowser:'/usr/bin/chromium-browser'"`

Here is some code you can add to your .bashrc file. Once that becomes part of your environment (by sourcing the file or by logging out and back in again), after loading SAS you can start SAS so that it can open the browser if needed.

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
