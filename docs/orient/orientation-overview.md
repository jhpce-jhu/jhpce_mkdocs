---
title: "Orientation Overview"
tags:
   - topic-overview
   - in-progress
---
# **About orientation**
All new cluster users are required to attend a JHPCE orientation session. These sessions
are generally held every other Wednesday afternoon via Zoom.

Before signing up for a sesssion, please be sure that you have completed the [New User Form](../joinus/new-users-form.md)
and that your PI has approved your account. During the signup process, you will be able to select a
time for your orientation session.

## **Documentation used in orientation**
You can download the orientation slides at [This Link](https://docs.google.com/presentation/d/1elMSTUdKws7FLVFK7vVV_AErA4brNSPX/pub){:target="_blank"}

We transitioned from the Sun Grid Engine to SLURM in 2024. We wrote [this document](../orient/images/transition-sge-2-slurm.pdf) for users familiar with SGE who needed to learn some SLURM basics.

## **What to do before the JHPCE Orientation Session**
Prior to attending the Orientation Session for the JHPCE cluster, you may need to
install some additional applications on you laptop or smart phone.

Please install this software prior to attending the JHPCE cluster orientation session.


### **Install the 2 Factor Authentication program**
The JHPCE cluster makes use of &#8220;Google Authenticator&#8221; to provide enhanced security. &nbsp;You can choose to either install an app on your smartphone or, if you do not have an Apple or Android based smart phone, you can install an extension to the Google Chrome browser.&nbsp; <strong>Prior to the Orientation Session, you will only need to download the Google Authenticator app on your smart phone, or install the Authy Chrome extension.</strong> We will be configuring Google Authenticator _during_ the Orientation Session.

### **Install required SSH client software**

You may need to install a couple of programs on your laptop or desktop in order to access the JHPCE Cluster. You will need:

1.  an **SSH client** for logging in,
2. an **SFTP client** for transferring files to and from the cluster, and
3. an **X11 server** for displaying graphics back from the JHPCE cluster.

The SSH client is a requirement; the SFTP and X11 clients are optional. For more information about SSH, see our page [here](https://jhpce.jhu.edu/access/ssh/)

#### **Microsoft Windows Users** 

We have found that the easiest program to use for accessing the JHPCE cluster is MobaXterm as it combines the functionality of all 3 software packages (SSH, SFTP, and X11) in 1 program.&nbsp; Prior to the Orientation session, you should install MobaXterm by following the first few steps of [https://jhpce.jhu.edu/access/mobaxterm/](https://jhpce.jhu.edu/access/mobaxterm/) .&nbsp; Alternatively, if you already use an SSH client, (such as <a title="putty" href=" http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html">putty</a> or <a href="http://x.cygwin.com/">Cygwin) </a>and an SCP client&nbsp; (such as <a href="http://winscp.net/eng/docs/free_sftp_client_for_windows">WinSCP), </a>you can continue using that software.</li>


#### **Apple Macintosh Users**

- SSH Client - There are built in command line tools for ssh and scp that can be run from a Terminal window.&nbsp; The Terminal program can be found in &#8220;Applications -&gt; Utilities&#8221;.&nbsp; From a Terminal window, you would type:
```ssh <username>@jhpce01.jhsph.edu```
and then login with the login id and the password we provided to you.
- Graphical Programs, X11 server - In order to run graphical programs on the cluster and have them displayed on your Mac, you will need to install **XQuartz** from <a href="http://xquartz.macosforge.org/landing/">http://xquartz.macosforge.org/landing/</a>.
- File Transfer - MacOS has a built in ```sftp``` text-based program for doing file transfers to and from the JHPCE cluster. Optionally, you can also install a GUI based SFTP program such as [Cyberduck](https://cyberduck.io/)

## **The JHPCE cluster is Linux Based**

If you have never used a Linux or Unix system before, we strongly recommend going through the Unix Command Line tutorial offered at the Digital Ocean site at&nbsp;<a href="https://www.digitalocean.com/community/tutorials/a-linux-command-line-primer" target="_blank" rel="noopener">https://www.digitalocean.com/community/tutorials/a-linux-command-line-primer</a>&nbsp;. The cluster is entirely Linux based, so some exposure to the Linux command line environment is recommended before attending the Orientation Session. The tutorial should only take 30 minutes or so to go through.</p>

## **Best practices - passwords and authentication**
Do not share your password with ANYONE.
Choose a &#8220;good&#8221; password using special characters and letters and digits.
It would be best if your password was unique and not the same password you use on other
systems. If you believe your password or your computer have been compromised please let
us know immediately so we can reset your password. If you store passwords on your
computer (not recommended), please let us know immediately if your computer is lost
or stolen.

## **Finally**
Hopkins staff will *NEVER* send you an email message asking for your password or
login credentials *NEVER* give out your password and login ID to anyone in an
email message or on a web page.
