Running Rstudio Server on the JHPCE cluster
===========================================

!!! Note "Authoring Note"
    A revised and updated version of this document exists at this [page](../sw/r-n-friends.md#running-rstudio-server).
    It has many text additions and clarifications as well as simple improvements like page sections marked with the correct markdown notation. It needs review to ensure that all of the image files needed are present, the version of the script to choose is the right one, etc. 
 
Rstudio Server is a web based environment for developing R programs.  On the JHPCE cluster we have put together a script called “jhpce-rstudio-server” which will allow you to run your own personal copy of Rstudio Server and access it from a browser on your laptop or desktop.  When the “jhpce-rstudio-server” program is run, it starts an instance of the Rstudio Server web server within a Singularity image on a unique port number, and then provides instructions for setting up an ssh tunnel to allow you to access Rstudio Server from your local system.  

To use Rstudio Server, start by srun-ing into a compute node, then run “jhpce-rstudio-server”, and then follow the  steps displayed. There are several versions of Rstudio Server installed, so you should run the appropriate program to pick the version that corresponds to the version of R that you want to use.  With the migration to SLURM, we currently have one version of R available in “jhpce-rstudio-server”.  If you just run jhpce-rstudio-server, you will be using R 4.3. The versions currently available, as of August 2024, are:

jhpce-rstudio-server
jhpce-rstudio-server-R4.3.0
jhpce-rstudio-server-R4.3.2

For CSUB users, use:
csub-jhpce-rstudio-server-R4.3.0

An example of running “jhpce-rstudio-server” is provided below.

```
[login31/users/jdoe1] $ **srun --pty --x11 --mem=10G bash**
Last login: Wed May 1 17:02:29 2019 from login31.cm.cluster
Loading conda_R/3.5.x
[compute-012 /users/jdoe1] $ **jhpce-rstudio-server**
------------
1) Establish your SSH tunnel.
* Windows users see:  https://jhpce.jhu.edu/sw/rstudio-server/
* Mac or Linux Desktop users, please type:
~C
-L 12345:compute-012:12345

2) From the browser running on your local system or SAFE desktop, go to:

  http://localhost:12345

3) When prompted for Username, enter your JHPCE ID:  jdoe1
4) When prompted for Password, enter your JHPCE password.

When you are finished using Rstudio Server, type <CTRL>-C

**~C**
ssh> **-L 12345:compute-012:12345**
Forwarding port.
```

You will need to perform one step to enable access to this Rstudio Server from your local laptop/desktop;  specifically, you will need to add a tunnel to your existing ssh session to the JHPCE cluster.

**For Mac or Linux laptops/desktops:**

To add this tunnel, first type **~C** (while holding dhown SHIFT, press “~” then “C”).  The **~C** is used to send an interrupt to your ssh session.  The **~C** will likely not show up, but you should see an “ssh>” prompt as a result.  At this “ssh>” prompt you activate the tunnel by typing  **\-L XXXXX:compute-YYY:XXXXX**  .  This will allow your laptop/desktop to access the compute node comptue-YYY on port XXXXX (in the above example, the port used was 12345 and the compute node used was compute-012).

**For Windows based laptops/desktops, or from the SAFE Desktop:**

If you connected to the JHPCE cluster with MobaXterm from a Windows-based system or SAFE desktop, you should ignore the first step (running ~C and adding the local tunnel).  Instead, you will need to  add a tunnel from MobaXterm.  Before setting up the tunnel you may find it helpful to set up an SSH key using the steps at [this page about ssh](../access/ssh.md#ssh-keys) While not a requirement, this will eliminate the need to login using your password and Google Verification Code.  Note that if you are setting up the tunnel for the CSUB, you will not be able to use SSH keys due to the enhanced security of the CSUB.

To start, click on the “Tunneling” icon at the top of MobaXterm, and you should see the window below:

![](images/Screen-Shot-2019-05-28-at-4.05.48-PM-3.png)

Click on “New SSH Tunnel”, and you should see:

![](images/Screen-Shot-2019-05-29-at-11.48.46-AM.png)

Enter the following in the window:

*   For “Forwarded Port”, enter the port number displayed (this is 12345 in the example)
*   For “Remote Server”, enter the compute node (compute-012 in the example)
*   For “Remote Port”, enter the port number (12345 in the example)
*   For “SSH Server”, enter “jhpce01.jhsph.edu” if you are on the JHPCE cluster, or “jhpcecms01.jhsph.edu” if you are on the CSUB.
*   For “SSH Login”, enter your JHPCE cluster login name (jdoe1 in the example)
*   For “SSH Port”, enter “22”

Then click “Save”, and you’ll be take back to the “MobaSSHTunnel” screen with your new tunnel displayed.

![](images/Screen-Shot-2019-05-28-at-4.08.55-PM.png)

Next, click on the yellow “Key” icon  [![](https://jhpce.jhu.edu/wp-content/uploads/2019/05/Screen-Shot-2019-05-29-at-12.48.29-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/05/Screen-Shot-2019-05-29-at-12.48.29-PM.png) and browse to the location of your private key (the “.ppk” file).  Now start your tunnel by clicking on the Green triangle icon[![](https://jhpce.jhu.edu/wp-content/uploads/2019/05/Screen-Shot-2019-05-29-at-12.48.20-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/05/Screen-Shot-2019-05-29-at-12.48.20-PM.png) in the “Start/Stop” column.

Now that the tunnel is established, you can then access Rstudio Server from your laptop/desktop by using a web browser, and connecting to the url, **http://localhost:XXXXX** .  Once connected you will be prompted for your login and password, and you will need to enter you your JHPCE login and password at this point.

![](images/Screen-Shot-2019-05-28-at-3.01.05-PM.png)

Once you enter your login and password, you should see Rstudio running in your browser.

![](images/Screen-Shot-2019-05-28-at-3.02.11-PM-1.png)

If you are accessing the cluster from a Windows based system, you would need to take an extra step to set up the port forwarding.

Shutting down the Rstudio Server
--------------------------------

When you have finished using Rstudio Server, you should close the browser tab or window that you are using to run Rstudio Server, and then return to the ssh session where you ran the “jhpce-rstudio-server” command.  To stop the Rstudio Server, type “^C”.  You will then be given a few additional steps to run to deactivate the port forward. As with the establishment of the tunnel, these steps are for MacOS and Linux based desktops/laptops.  You will again be prompted to type “~C”, and then enter “-KL XXXXX” at the “ssh>” prompt to stop the forwarding (NOTE: you’ll need to hit <enter> once before typing “~C”).  The session should look similar to:

```console
[login31 /users/jdoe1] $ srun --pty --x11 --mem=10G bash
Last login: Wed May 1 17:02:29 2019 from login31.cm.cluster
Loading conda_R/3.5.x
[compute-012 /users/jdoe1] $ jhpce-rstudio-server
------------
1) Establish your SSH tunnel.
* Windows users see: https://jhpce.jhu.edu/sw/rstudio-server/
* Mac or Linux Desktop users, please type:
~C
-L 12345:compute-012:12345

2) From the browser running on your local system or SAFE desktop, go to:

  http://localhost:12345

3) When prompted for Username, enter your JHPCE ID:  jdoe1
4) When prompted for Password, enter your JHPCE password.

When you are finished using Rstudio Server, type <CTRL>-C

~C
ssh> -L 12345:compute-012:12345
Forwarding port.

****Cleaning up...
------------
Now type:
~C
-KL 41354

[compute-012 /users/jdoe1] $
[compute-012 /users/jdoe1] $**~C**
ssh> **-KL 12345**
Canceled forwarding.
[compute-012 /users/jdoe1] $ exit
[login31 /users/jdoe1] $ exit
```

For Windows desktops/laptops, you should also use “^C” to terminate the Rstudio Server, but to stop the tunnel you will need to return to the MobaSSHTunnel screen, and use the “Stop” icon [![](https://jhpce.jhu.edu/wp-content/uploads/2019/05/Screen-Shot-2019-05-29-at-1.46.04-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/05/Screen-Shot-2019-05-29-at-1.46.04-PM.png) in the Start/Stop column.  You can keep this tunnel configuration in MobaSSHTunnel, and reuse it the next time you run Rstudio Server, however you will need to edit the tunnel configuration and change the “Remote Server” to match the compute node you are running on.

FAQs/Comments
-------------

* Q) Why did you do this?  R works just fine for me on the cluster!  *
A) On the JHPCE cluster we have historically had several ways to run R programs.  Often  people will use the text-based version of R to run programs, and that works well for a lot of people.  Some people prefer to work in a graphical environment, so we also have the X11-based “Rstudio” available on the cluster, which is great, except that on a slower network connection, this can get quite laggy.  The web based Rstudio Server provides the same graphical version available in Rstudio, but over a much lighter network protocol than the X11-based Rstudio, so it is much faster and more responsive to use.

*** Q) Why not just set up a dedicated web server to run Rstudio Server like I had back at ZZZZ?  ***
A) Rstudio Server does not play well with clusters.  For us to run a dedicated Rstudio Server server, we would need to purchase a fairly large system with lots of RAM and CPU power.  This was considered, but in the end was deemed cost prohibitive, and it didn’t allow the use of the JHPCE cluster resources to run R programs.  This solution allows the nice web-based Rstudio Server to be used, while making use of the existing CPU and RAM resources available on the cluster.

Q) My program can’t run because it needs XXX package!  
A) The R that is run within the Rstudio Server is completely separate  from the default version of R that is used on the JHPCE cluster, therefor you may need to install packages using the install.packages() function, or through the Rstudio Server GUI .

Q) I forgot to cleanly disconnect from the Rstudio Server/My session got disconnected.  
A) This should be fine.  Your interactive srun session will eventually time out and will kill the Rstudio Server that was running.  You may get warning messages about ports being in use – if so, please wait a few minutes and try again.

Q) When I try to add the port forward, I get an error message about “Port is in use”.  How do I fix this?  
A) You can only run one instance of Rstudio Server. You likely have another SSH session running that has the port forward lingering.  If you had been using Rstudio Server in another SSH session, you will need to either need to log out of that ssh session, or run the “~C” “-KL XXXXX” command to tear down the port forward.

If you have any questions  about using Rstudio Server, please feel free to email us at bitsupport.
