---
tags:
  - in-progress
  - adi
hide:
  - toc
---
# JHPCE Web Enabled User Apps

[![cluster logo](/images/web_portal.png)](https://jhpce-app02.jhsph.edu)

Our [web portal](https://jhpce-app02.jhsph.edu/) has several sections. ==You will need to log with your JHED ID and password. This web site is only available on campus, so if you are outside of the school network, you will need login to the JHU VPN first.==



<div class="grid cards" markdown>
  
-   :material-information-outline:{ .lg .middle } __JupyterLab__
  ![cluster logo](/images/jhpce_jupyter.png)]
    ---

    JupyterLab is the latest web-based interactive development environment for notebooks, code, and data. Its flexible interface allows users to configure and arrange workflows in data science, scientific computing, computational journalism, and machine learning. A modular design invites extensions to expand and enrich functionality. 

-   :material-information-outline:{ .lg .middle } __RStudio__
    ![cluster logo](/images/jhpce_R_studio.png)]
    ---

    RStudio is an integrated development environment (IDE) for R. It includes a console, syntax-highlighting editor that supports direct code execution, as well as tools for plotting, history, debugging and workspace management.

-   :material-information-outline:{ .lg .middle } __Visual Studio Code__
    ![cluster logo](/images/jhpce_Visual_studio.png.png)]
    ---

    Visual Studio Code is a lightweight but powerful source code editor. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages and runtimes (such as C++, C#, Java, Python, PHP, Go, .NET). 
  
</div>

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


