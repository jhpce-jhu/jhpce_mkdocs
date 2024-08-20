---
tags:
  - needs-major-revision
---

# General Tips/Requests

## The cluster is a shared resource

Generally expected knowledge
+ Take care not to use too many resources on the login nodes. Anything CPU, RAM, or input/output intensive should be done on compute nodes rather than one of the login nodes.
+ Anything more than a quick `ls` including: copying large files, recursively changing permissions, creating or extracting tar or zip archives, running a `find` should be done in a session on a compute node.
+ Data transfers of files larger about 1GB should be done through `jhpce-transfer01.jhsph.edu` rather than a login node.
+ Try to avoid having directories with more than 100 files in them. 
+ Try to avoid storing programs and scripts in data directories like `DCL*`.
+ Most storage on the cluster is raided but not backed up. 
+ We do back up home directories and a few other select directories on the DCS and DCL systems for groups that have requested backups.  We do have additional backup storage capacity available for a small fee.
+ Make use of your 1 TB of fastscratch storage for IO intensive job
+ Please remember that DCS and DCL stand for “Dirt Cheap Storage” and
  “Dirt Cheap Lustre”, and were designed with cost-effectiveness as a
  primary driving factor over performance.
+ Sharing data can be done in several ways on the cluster: i.) traditional Unix file permissions and groups and ii.) Access Control Lists (ACLs).
+ Sharing files with external collaborators can be done via Globus.

## Best practices: passwords and authentication
+   Do not share your password with ANYONE. You are responsible for the use of your account.
+   Choose a "strong" password.
+   Use the "kpasswd" command to choose a new password. It requires your new password to have three of the following four sets of
characters: upper-case, lower-case, numerical digits, and special characters.
+   It would be best if your password was unique and not the same
    password you use on other systems.
+   If you believe your password or your computer have been compromised
    please alert us via email to bitsupport@lists.jh.edu. Then reset your password using a different device. Visit
    https://jhpce-app02.jhsph.edu/  to reset your password.  This web
    site is only available on campus, so if you are outside of the
    school network, you will need login to the JHU VPN first. You will
    log into that page with your JHED ID and password.
+   Hopkins staff will \*NEVER\* send you an email message asking for
    your password or login credentials
+   **NEVER** give out your password and login ID to anyone in an email
    message or on a web page.

If you have never used a Linux or Unix system before, we strongly
    recommend going through the [Unix Command
    Line](https://www.digitalocean.com/community/tutorials/a-linux-command-line-primer%20)
    tutorial. The cluster is entirely Linux based. Our Orientation is
    about using the cluster, not using Linux. This tutorial should
    only take 30 minutes or so to go through.
    
## New C-SUB user orientation 

### What to do BEFORE the C-SUB JHPCE Orientation Session 

**There is a lot of material for us to cover and you to absorb. It is
vital for your success that you complete a number of steps PRIOR** to
attending the Orientation Session for the CMS Subcluster of the JHPCE
(pronounced by its letters J-H-P-C-E) cluster.

+   Download a copy of the slides from the Orientation
    from:[JHPCE-Overview-CMS.pdf.](http://jhpce-old.jhu.edu/wp-content/uploads/2024/01/JHPCE-Overview-CMS-2023-12.pdf)
+   
+   In order to access the JHPCE cluster and make use of the
    applications on the cluster, you may need to install additional
    software on your smart phone and laptop.

1.  **Install the 2 Factor Authentication program.**  The JHPCE cluster
    makes use of "Google Authenticator" to provide enhanced security.
     You can choose to either install an app on your smartphone or, if
    you do not have an Apple or Android based smart phone, you can
    install an extension to the Google Chrome browser.  **Prior to the
    Orientation Session, you will only need to download the
    GoogleAuthenticator app on your smart phone, or install the Authy
    Chrome extension.** We will be configuring Google Authenticator
    during the Orientation Session. 
2.  **Install required client software.**  You may need to install a
    couple of programs on your laptop or desktop in order to access the
    JHPCE Cluster.  You will need 1) an SSH client for logging in, 2) an
    SFTP client for transferring files to and from the cluster, and 3)
    an X11 client for displaying graphics back from the JHPCE cluster.
     The SSH client is a requirement -- the SFTP and X11 clients are
    preferable but optional.
    +   **Microsoft Windows** We have found that the easiest program to use for accessing the
        JHPCE cluster is MobaXterm as it combines the functionality of
        all 3 software packages (SSH, SFTP, and X11) in 1 program. 
        Install MobaXterm by following the first few steps of
        <https://jhpce.jhu.edu/access/mobaxterm.md>
        .  Alternatively, if you already use an SSH client, (such as
        [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
        or [Cygwin)](http://x.cygwin.com/) and an SCP client  (such as
        [WinSCP),](http://winscp.net/eng/docs/free_sftp_client_for_windows)
        you can continue using that software.
    +   **Apple Macintosh** There are built in command line tools for ssh and scp that
        can be run from a Terminal window. The Terminal program can be
        found in "Applications -\> Utilities". From a Terminal window,
        you would type:\
        `ssh <username>@jhpcecms01.jhsph.edu `and then login with
        the login id and the password we provided to you.- In order to
        run graphical programs on the cluster and have them displayed on
        your Mac, you will need to install XQuartz from
        <http://xquartz.macosforge.org/landing/>.- Optionally, you can
        also install a GUI based SFTP program such as
        "[Filezilla](https://filezilla-project.org/)". One note about
        Filezilla -- if you download the package from the default link
        on SourceForge, you may be be blocked by your MalWare/Virus
        Scanner, or prompted to install Potentially Unwanted Programs
        (PUPs) during installation.  We recommend you follow the
        alternative download link
        [here](https://filezilla-project.org/download.php?show_all=1) to
        download a clean copy of the program.

