# Run code-server interactively

### Overview
We do not allow VS Code or Code-server to be run on login nodes. Therefore users need to log in, get onto a compute node and run the application there.

We have two methods to do this:

* Entirely manually, which works on both clusters and is described in this document.

* Use a web portal. Our main cluster has a web portal ([https://jhpce-app02.jhsph.edu](https://jhpce-app02.jhsph.edu)). Our JADE cluster has an Open OnDemand server [https://jade-ondemand01.jhsph.edu](https://jade-ondemand01.jhsph.edu).  Using these web interfaces requires several things to be true:

    1. You must be on a Hopkins network (which includes the VPN).
    2. You must have an active JHEDID to log into the portal (and to log into the VPN if you're "off campus").
    3. For the main cluster web portal: Your Hopkins JHEDID account has to be configured to know the text-message-capable phone number to send job-specific information to you (e.g. the name of the compute node it chose to launch your session on).

 
### Details

!!! warning "Customize your commands!"
    The following examples show the approach you should follow. Do not use the details shown -- you must substitute in your specific information, such as the name of the cluster login server, your cluster username (here we use "test"), the name of the compute node that is assigned to your job by SLURM and the TCP/IP port number assigned (here "40000").
   
1. Run the following commands after logging into either cluster.

```shellsession
[test@jhpce01 ~]$ srun --pty bash
[test@compute-149 ~]$ codeserver.sh 
        ==================================================================================
        Starting code-server

        Please type the following on your local terminal
            ## Note: Windows users, please use PowerShell
            ssh -J test@jhpce01.jhsph.edu -N -L 40000:127.0.0.1:40000 test@compute-149

        Then open:
        http://127.0.0.1:40000

        ====================================================================================
[2026-05-04T19:43:21.633Z] info  code-server 4.112.0 d7599ae360900ad55b503e3c840b417a1eae4798
[2026-05-04T19:43:21.646Z] info  Using user-data-dir /users/test/.local/share/code-server
[2026-05-04T19:43:21.662Z] info  Using config file /users/test/.config/code-server/config.yaml
[2026-05-04T19:43:21.662Z] info  HTTP server listening on http://127.0.0.1:40008/
[2026-05-04T19:43:21.662Z] info    - Authentication is disabled
[2026-05-04T19:43:21.662Z] info    - Not serving HTTPS
[2026-05-04T19:43:21.662Z] info  Session server listening on /users/test/.local/share/code-server/code-server-ipc.sock
```

2. Set up port forward - open a terminal on your local machine, copy and paste line from the output of `codeserver.sh`. 

```shellsession
ssh -J test@jhpce01.jhsph.edu -N -L 40000:127.0.0.1:40000 test@compute-149
```

3. Open a browser on your local machine, then copy and paste the URL from the output of your `codeserver.sh`

```
http://127.0.0.1:40000
```

The code-server interface should now appear in your local browser.

4. Once you are done, type ++CTRL++-c in your cluster terminal and then in your local terminal, and close the browser.

!!! Note "Issues you might run into"
    (1) Note that we use `http` in the URL, not `https`. Your web browser may try to use https.
    (2) You may find that this method works better in one web browser than another. The security rules built into browsers might make things either break or be more troublesome.
    (3) If your session fails, whether you are able to successfully connect or it seems to die in mid-stream, please check to see whether the job is still running. You can use `squeue` to check for your jobs on the workstation in question. If your code-server job appears, please use `scancel <jobid>` to kill it. Otherwise it will continue to run for a full day, which consumes community resources as well as costing you money.

