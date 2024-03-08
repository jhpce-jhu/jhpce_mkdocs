---
tags:
  - in-progress
  - adi
---
# JHPCE User Apps

Our [web portal](https://jhpce-app02.jhsph.edu/) has several sections.

The JHPCE User Apps section provide the means to run a growing number of applications on the cluster via a web interface.

Currently supported:

* JupyterLab ([our page](../sw/jupyter.md) about using it)
* RStudio ([our page](../sw/r-n-friends.md) about using it)
* Visual Studio Code ([our page](../sw/vscode.md) about using it)

!!! Note "Authoring Note"
    What do users need to know about using this service in practice?
    
    How long do sessions run if you don't connect to them?
    Do they automatically time out?
    
    Do users need to do anything to clean up if they change their mind or cannot connect to it or things freeze up? (`squeue --me` and `scancel jobid`?)
    
    Can you disconnect from and then reconnect to any of these apps?


