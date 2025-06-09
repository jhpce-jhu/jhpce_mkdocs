## Introduction
JHPCE has installed the [Google
Chromium](https://www.chromium.org/chromium-projects/) web browser. It is launched with the command: `chromium-browser` This Linux
software is a version of the Google Chrome web browser found on other operating systems.[^1]
[^1]:
(Our web site uses the terms Chrome and Chromium interchangeably. But it is
actually Chromium. The process names shown in `ps` output include "chromium-browser")

(We removed the Firefox browser from the cluster because it performed very poorly
in  recent versions of the RHEL 9-based Rocky operating system that we used.)

This page describes some solutions and recommendations for our users.

## Locked profiles
It is not uncommon that cluster users run into the following error message when they try
to launch Chrome:

```
The profile appears to be in use by another Chromium process (199919)
on another computer (compute-102.cm.cluster). Chromium has locked the
profile so that it doesn't get corrupted. If you are sure no other 
processes are using this profile, you can unlock the profile and
relaunch Chromium.
```
(The process number and computer name will be different in each case.)

{==The way to avoid this problem is to always quit Chrome properly before your
SLURM interactive session ends.==} Do that by choosing "Exit" from the drop down menu revealed by clicking on the three vertical dots in upper right corner. See [this image](../images/exiting-chromium-browser.png)

### Solving the locked profile problem

See the explanation in the next section if you want to know why you need to take these actions.

You need to 


1. quit your Chromium session
2. if Chromium was launched as a part of starting SAS, then quit the SAS application as well (because it has created an internal reference to that particular Chromium process)
3. then delete the three files which comprise the "lock":

```bash
cd ~/.config/
/bin/rm chromium/Singleton*
```

#### Still having issues starting Chrome?
If that does not resolve the problem, then you may need to delete the Chromium directories

```
/bin/rm -rf ~/.cache/chromium
/bin/rm -rf ~/.config/chromium
```

!!! Warning "Warning"
    Deleting the `~/.config/chromium` directory will delete your browsing history, bookmarks, etc. This is often okay in our environment, but you know how you use Chrome. You can instead rename the profile directory if worried about losing material in there. However, if that solved the "profile locked" problem, then retrieving any of the material in the renamed directory and placing it into your new profile's directory may involve some complex steps. If you customize Chrome, you may want to create a specific non-default profile for that.

You can use this command:

```bash
mv ~/.config/chromium ~/.config/chromium-locked
```

There is a possibility that the Chromium process mentioned by the "profile
locked" error message is still running even though your SLURM interactive job
has ended. If that happens then that browser session might re-create the "Singlton" files. We haven't seen that occur. If it did, then, substituting in your specific process id and 
compute node name, 

1. Log into that node `srun --pty --nodelist=compute-102 bash`
2. Look for that process id number `ps -f -p 12345`
3. If you see a process listing where your name is listed under UID, and
"chromium-browser" under the CMD header, then you should kill it. If you don't
see anything, then the process mentioned by the "profile locked" message died.
4. Kill that process by running `kill -KILL 12345`
5. After a few seconds delay, do that again.
6. You should receive the message `12345: No such process"

### What causes this error?

All web browsers maintain, in a user's home directory, an extensive set of files
supporting bookmarks, browsing history, extensions, and possibly multiple user profiles.

On Linux systems, this directory is named `~/.config/chromium/`

By default there is a single profile. The information for that profile is stored
in a subdirectory of the primary directory, e.g. `~/.config/chromium/Default/`

The UNIX environment at many organizations contain multiple networked computers 
which share a single user home directory. (That directory is stored on a storage
server and shared to select other machines.) Therefore, the authors of
Chromium had to accomodate the possibility that the same home directory would
be asked to support Chromium use on multiple computers. They chose to support
use by a single profile on a single computer at a time. 

When Chromium is launched, it creates a lock file within the profile directory
which records the computer hostname, the process id ("pid") on that computer,
the date, etc. This lock file is removed if one quits Chrome by choosing Exit
from Chromium's menu (see [this image](../images/exiting-chromium-browser.png)).

If your interactive session ends unexpectedly (e.g. runs out of memory) or you 
exit your shell without first quitting Chromium, then the browser process is 
killed off without it having the time to clean up after itself, by, among other
things, removing the lock file. That file remains -- whether or not the processes
associated with the browser are still running.

### Running multiple concurrent Chromium sessions

If all you want to do is to have multiple browser windows, you can choose "New Window" from the same drop-down menu that Exit is found on.

But if you wanted to run Chrome on two different compute nodes simultaneously, then you would need to create a second profile. [This image](../images/chrome-create-new-profile.png) shows where to do that. Then launch each session with different command-line
arguments specifying a different profile name. This places the files for that particular
session in a different subdirectory of `~/.config/chromium/`

First, in an existing Chromium session, you would create a new profile. Try to
specify profile names using hyphens instead of spaces, as UNIX does not handle
spaces in file names without the user enclosing them in quotes or escaping them
with the backslash character.

Second, you would launch a new browser using that profile with a command like:

`chromium --profile-directory=Profile-1`
or
`chromium --profile-directory="Profile 1"`
or
`chromium --profile-directory="Profile\ 1"`


## Chromium spews out error messages when launched

Web browsers are normally run on a system sitting physically in front of you.
Those are equipped with a graphics card. Our compute nodes don't have such cards or the software to support them.
When Chromium launches, it doesn't find that card and software and emits a
stream of warnings which you can ignore.

You can avoid seeing them if you redirect standard output and standard error
data streams. This is entirely optional.

`chromium-browser /dev/null 2>&1`

!!! Warning "Warning"
    Redirecting all I/O means that if SAS itself emits important error messages, you won't see them. So don't redirect I/O if SAS isn't working as expected! Maybe SAS is trying to tell you that you specified a file name that doesn't exist, for example.

## Handy bash routine to add to your environment

Many of our users rely on Chrome to enable functionality in SAS sessions. See [this section](https://jhpce.jhu.edu/sw/sas/#with-browser-support) for the command you need to issue. There is also a bash routine named `csas` that you can add to your `~/.bashrc` file which will allow you to start SAS with one word. (You can name it whatever you want instead of `csas`.)

{==To ensure that you avoid the "locked profile" issue, you should explicitly exit Chrome before ending your SAS session, or the interative SLURM session that SAS and the browser are running within.==}
