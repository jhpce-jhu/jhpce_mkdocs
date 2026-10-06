# Virtual Studio Code

The currently only acceptable way to run vscode on the cluster is via the
JHPCE Web Portal. By default, running VSCode with the normal "Remote-SSH"
extension will run on the JHPCE login node, and this puts undue stress on
the login node.

## RStudio Server
If you’re trying to run a Singularity image program like RStudio Server which requires creating an SSH tunnel, you cannot issue a ++ctrl+c++ key combination from within the terminal window of VSCode. That’s (almost certainly) not going to work.  The ++ctrl+c++ sends an interrupt back to your **original** ssh connection from your laptop to the JHPCE login node.  Since you’re using vscode to make that connection, it’s not a “real” ssh session, so it won’t be able to interpret the ++ctrl+c++.  We don’t believe there is a way around that… but if you find one please let us know.

More information is at [https://jhpce.jhu.edu/portal/web-apps/](https://jhpce.jhu.edu/portal/web-apps/)
