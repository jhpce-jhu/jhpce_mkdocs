# FAQ

+ **Why does bash report that it can’t find the module command?** If you receive a message like
    
    ```bash
    bash: module: command not found
    ```
    
    The module is a shell function that is declared in `/etc/bashrc`. It is always a good idea for `/etc/bashrc` to be sourced immediately in you `~/.bashrc`.  Edit your `.bashrc` file so that the first thing it does is o execute the system bashrc file, i.e. your `.bashrc` file should start with the following lines:

    ```bash
    if [ -f /etc/bashrc ]; then
        . /etc/bashrc
    fi
    ```

+ **My script is giving odd error messages about “\r” or “^M”.** Windows and Unix use different characters to indicate a new line.  If you have uploaded your script from a Windows machine, it may have the Windows newline characters.  These need to be replaced by the Unix newline characters.  To do this, you can run the “dos2unix” command on your script `dos2unix myscript.sh`. This will strip out all of the Windows newlines and replace them with the Unix 

+ **I’m getting X11 errors when using rstudio with Putty and Xming** We’ve had issues reported by users of Putty with Xming. One solution we’ve found is to use the (vcxsrv))[https://sourceforge.net/projects/vcxsrv/] instead  Xming
newlines.  (This commmand is on all nodes.)


+ **How can I add packages into emacs on the cluster?** Make sure to `module load emacs` to get a later version of emacs. Then one needs to edit their `.emacs` file in their home director to include the package repos
    ```emacs
    (require 'use-package)
    (add-to-list 'package-archives '("gnu" . "http://elpa.gnu.org/packages/") t)
    (add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)
    ```
    After restaring emacs then one can do `M-x package-list-packages` and follow the GUI.


+ **Xauth error messages from MacOS Sierra when using X11 forwarding in SSH** With the upgrade to MacOS Sierra, the “-X” option to ssh to enable X11 forwarding may not work.  If  you receive the message: `untrusted X11 forwarding setup failed: xauth key data not generated` , you can resolve the issue by add the line `ForwardX11Trusted yes` to your `~/.ssh/config` file on your Mac. You may still see the warning: Warning: `No xauth data; using fake authentication data for X11 forwarding.` To eliminate this warning, add the line `XAuthLocation /usr/X11/bin/xauth` to your `~/.ssh/config` file on your Mac.

+ **When running SAS, an error dialog pops up about Remote Browser** In SAS, results are sometimes presented in HTML format, and need a web browser running in order to view the results. When SAS is used on a desktop, the local desktop browser is used. However when SAS is run on a remote system, such as on the JHPCE cluster, SAS doesn’t know how to connect to the browser, and needs to use the SAS Remote Browser utility. To do so:

    1) Login to the cluster as you normally would:
    2) On the jhpce01 login node, start up the Firefox browser in the background:
        `console> firefox &`
    3) Still on jhpce01,
        `console> rbrowser &`
    4) Now connect to the SAS queue, and start up the SAS program.
        ``` 
        console> qrsh -l sas
        console> sas
        ```
    You will be prompted to allow pop-ups from the SAS node when you first try and display something back to the Firefox browser on jhpce01 – you should allow the pop-ups.


