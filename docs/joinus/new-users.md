---
tags:
  - needs-major-revision
---

# THIS DOCUMENT NEEDS TO BE REWRITTEN

## What does a new user need to know, as an overview
That's what JRT envisions for this page. Short, sweet, how does the process work and how long will it be before you can login.

## What should be here versus in the orient topic?
JRT created a topic subdirectory docs/orient/ to hold orientation-related pages. 


JRT didn't put the c-sub info in this document.
(He was assuming that c-sub users would have their own page.)
In any case, maybe some of the c-sub material should be made a part of this page.

# The Process

If you are prospective user, please start by getting the approval of a
PI who has provided us with a budget number. (If you and they are both new to JHPCE, please ask them to visit [this page](new-pi.md).) 
Then request a user
account by filling out [this
form](https://jhpce.jhu.edu/register/user/).  Once we have received
your form, the JHPCE management team will contact you to schedule your
attendance at the next JHPCE Orientation.  All new users are required
to attend a 1 1/2 hour orientation session. Orientations are generally
scheduled every other week.

## Orientation

There is a [separate document](../orient/orientation.md) for orientation. JRT created a topic subdirectory docs/orient/ to hold orientation-related pages. 


## New C-SUB user orientation 

### What to do BEFORE the C-SUB JHPCE Orientation Session 

**There is a lot of material for us to cover and you to absorb. It is
vital for your success that you complete a number of steps PRIOR** to
attending the Orientation Session for the CMS Subcluster of the JHPCE
(pronounced by its letters J-H-P-C-E) cluster.

+   Download a copy of the slides from the Orientation
    from:[JHPCE-Overview-CMS.pdf.](https://jhpce.jhu.edu/wp-content/uploads/2024/01/JHPCE-Overview-CMS-2023-12.pdf)
+   If you have never used a Linux or Unix system before, we strongly
    recommend going through the [Unix Command
    Line](https://www.digitalocean.com/community/tutorials/a-linux-command-line-primer%20)
    tutorial. The cluster is entirely Linux based. Our Orientation is
    about using the cluster, not using Linux. This tutorial should
    only take 30 minutes or so to go through.
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
    during the Orientation Session. Please
    see <https://jhpce.jhu.edu/knowledge-base/authentication/2-factor-authentication/#otp>
    for instructions.
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
        <https://jhpce.jhu.edu/knowledge-base/mobaxterm-configuration/>
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

### Best practices: passwords and authentication

What should be said about MFA and SSH Keys?

+   Your JHPCE account username and password are not related to your JHED credentials. They are maintained by separate organizations. Changing one does not cause the other to change.
+   Do not share your password with ANYONE.
+   Choose a "strong" password.
+   Use the "kpasswd" command to choose a new password. It _requires_ your new password to be composed of at least 3 of the following 4 sets of characters: upper-case, lower-case, numerical digits, and special characters.
+   It would be best if your password was unique and not the same
    password you use on other systems.
+   If you believe your password or your computer have been compromised
    please reset your password using a different device. Visit
    https://jhpce-app02.jhsph.edu/  to reset your password.  This web
    site is only available on campus, so if you are outside of the
    school network, you will need login to the JHU VPN first. You will
    log into that page with your JHED ID and password.
+   Hopkins staff will \*NEVER\* send you an email message asking for
    your password or login credentials
+   **NEVER** give out your password and login ID to anyone in an email message or on a web page.
+   (Are there any cautions about leaving sessions running with tmux or screen?)


## Generally expected knowledge
+ Anything I/O intensive should be done on the compute nodes rather than the jhpce01 login node.
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
