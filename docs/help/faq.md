## Compatibility
- My script is giving odd error messages about `\r` or `^M`.

??? "Click to expand answer"
    Windows and Unix use different characters to indicate a new line.  If you have uploaded your script from a Windows machine, it may have the Windows newline characters.  These need to be replaced by the Unix newline characters.  To do this, you can run the “dos2unix” command on your script `dos2unix myscript.sh`. This will strip out all of the Windows newlines and replace them with the Unix newlines.

- I’m on a Mac, and the ~C command to interrupt an ssh session isn’t working
??? "Click to expand answer"
    New versions of MacOS have disabled the default ability to send an SSH Escape with `~C` (++tilde++ ++shift+c++). To reenable this, on you Mac, you need to set the `EnableEscapeCommandline` option. You can do this by either running `ssh -o EnableEscapeCommandline=yes . . .` or by editing your `~/.ssh/config` file, and at the top of that file add the line:
    ```
    EnableEscapeCommandline=yes
    ``` 
- My app is complaining that it can’t find a shared library, e.g. libgfortran.so.1
??? "Click to expand answer"
    Nine times out of ten, the allegedly missing library is there. The problem is that your application is looking for the version of the library that is compatible with the old system software. It will not help to point your application to the new libraries. They are more than likely to be incompatible with the new system. The correct solution is to reinstall your software. If the problem persists after the reinstallation, then please contact us and we will install standard libraries that are actually missing.

## File Transfer
- How do I copy a large directory structure from one place to another? Within the cluster? Into the cluster? Out of the cluster?

??? "Click to expand answer"
    We have a document about transferring data into or out of the cluster [here](../access/file-transfer.md).
    We have a document with advice about copying data around within the cluster [here](../files/copying-files.md). (This also includes good tips for using the rsync program for any data copying.)
    
    As an example, to copy a directory tree from /users/bob/src to /dcs08/bob/dst, first, create a cluster script, let's call it "copy-job", that contains the line
    ```
    #!/bin/bash

    rsync -avzh /users/bob/src /dcs08/bob/dst
    ```
    Next, submit a batch job to the cluster
    ```
    sbatch --mail-type=FAIL,END --mail-user=bob@jhu.edu copy-job
    ```
    This will submit the "copy-id" script to the cluster, which will run the job on one of the computer nodes, and send an email when it finishes.

    !!! Warning
        Please do not copy or move any larger files on the login nodes. Use the transfer node for internal/external transfers and compute nodes for transfers between cluster storage locations.

## Login
- SSH gave a warning: REMOTE HOST IDENTIFICATION HAS CHANGED
??? "Click to expand answer"
    Go into the ~/.ssh directory of your laptop/desktop and edit the known_hosts file. Search for the line that starts with the host that you ssh’d to. Delete that line (it is probably a long line that wraps). Then try again

- SSH "Bad owner or permissions"

??? "Click to expand answer"
    If you receive a message like "Bad owner or permissions on ~/.ssh/config" or continue to have to provide your password when ssh'ing to JHPCE when you think you have configured things to not need one, your file owner or permissions may be incorrect. See [this document](../access/ssh.md/#permissions-on-ssh-files) for answers.

- How do I delete saved passwords in MobaXterm?
??? "Click to expand answer"
    When using MobaXterm you should not save your password when prompted to do so. MobaXterm will save your password, and then inadvisedly try to use that as your “Verification Code:”, which means that when you first connect to the cluster in MobaXterm, you are prompted for “Password:”, and which you will need to press to be prompted for “Verification Code:” If you accidentally saved your password, you can remove the saved password by the following steps.  

    1) In MobaXterm, go to "Settings->Configuration"  
    ![Configuration Window](../images/MobaPass1.png)

    2) On the next screen, select "MobaXterm Password Management"  
    ![General Window](../images/MobaPass2.png)

    3) This will display a list of saved passwords, and you should delete all of the entries that reference “jhpce”  
    ![Password Settings](../images/Screen-Shot-2022-01-19.png)

    Once these entries are deleted, you should be prompted for “Verification Code:” when you connect to the cluster via MobaXterm.

## Modules
- Why does bash report that it can’t find the module command?

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

## Resources
- What is the SAFE desktop?

??? "Click to expand answer"
    The SAFE desktop is a virtual Windows computer that you can use to run scientific software and access JHPCE. For more information see this [item](../access/access-overview.md#safe-desktop).

## RStudio
- Why do I see a mass of error messages about "GL" and "GPU" every time I launch RStudio?

??? "Click to expand answer"
    Those errors are normal. RStudio expects to be run on a computer with an attached display driven by a local graphics processing card and supported by a set of graphics software libraries and device drivers. We don’t have such cards in our rack-mounted systems, nor the software that goes with them.

## Slurm
- There is a dedicated [SLURM FAQ](../slurm/slurm-faq.md) document

## SSH
- For a variety of questions about ssh - please see our [ssh document](../access/ssh.md).

## X11
- My X11 forwarding stops working after 20 minutes 
??? "Click to expand answer"
    This error comes from the `ForwardX11Timeout` variable, which is set by default to 20 minutes.  To avoid this issue, a larger timeout can be supplied on the command line to, say, 336 hours (2 weeks):

    ```
    $ ssh -X username@jhpce03.jhsph.edu -o ForwardX11Timeout=336h
    ```

- Xauth error messages from MacOS Sierra when using X11 forwarding in SSH

??? "Click to expand answer"
    With the upgrade to MacOS Sierra, the “-X” option to ssh to enable X11 forwarding may not work. If you receive the message: untrusted X11 forwarding setup failed: xauth key data not generated , you can resolve the issue by add the line ForwardX11Trusted yes to your ~/.ssh/config file on your Mac. You may still see the warning: Warning: No xauth data; using fake authentication data for X11 forwarding. To eliminate this warning, add the line XAuthLocation /usr/X11/bin/xauth to your ~/.ssh/config file on your Mac.
