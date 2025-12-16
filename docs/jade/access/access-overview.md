---
tags:
  - jade
  - topic-overview
title: "Access JADE Overview"
---

# THIS PAGE IS BEING DEVELOPED !!!!!!!!!!!!!!

# Accessing the JADE Cluster: Overview

This page was created by duplicating the [access overview page for the main cluster](access/access-overview.md)
and beginning to revise it.

## Cluster structure overview

JADE can only be accessed from Hopkins networks. JADE serves three user
communities. The dbGAP users must connect to JADE from the SAFE or SAFER
Desktops. 

Where members of each community can login from:

=== "CMS Users"
    Hopkins VPN & campus networks

=== "dbGaP Users"
    [SAFE Desktop](#safe-desktop) or [SAFER Desktop](#safer-desktop)

=== "Sysadmin Users"
    Hopkins VPN & campus networks


## Network Diagram

TODO: INSERT SCALED IMAGE HERE

## Multi-factor authentication (MFA)
There are two basic "factors" required to log into a computer, whether your
laptop or a remote UNIX cluster login node -- your username and a password.
{==JADE requires the use of an additional factor==}, a [one-time
password](ssh.md#one-time-passwords) (OTP) six digit code. 

, or the use of [SSH key pairs](ssh.md#ssh-keys). 

### One Time Passwords
When you SSH into JHPCE, you will be prompted for a “Verification Code:” This is your cue to enter in a one-time password six digit code.

Programs like `Google Authenticator` and `Micrsoft Authenticator` generate one-time password codes (OTP). These are only good for a single use, whether you successfully log in or not. Typically they are used to generate a _stream_ of time-based OTPs, or TOTPs. These are only good for one minute, adding another layer of difficulty for someone trying to impersonate you.

These programs are usually used on smartphones, but there are programs available to create them on laptops and desktops. The key with using ANY OTP program is to get it from a trusted source. We will default to mentioning the `Google Authenticator`.

After you log into JHPCE for the first time, you should immediately configure your OTP program using a "secret" accessible to you on the cluster via the `auth_util` program. Instructions for doing that are found in the [orientation documents](https://docs.google.com/presentation/d/1elMSTUdKws7FLVFK7vVV_AErA4brNSPX/pub){:target="_blank"}.


cluster consists of some public-facing hosts with the remaining computers "behind them" on private networks. An incomplete and very oversimplified approximation:

<!--

```mermaid
graph TD
   A[Your computer] --> B([Login nodes])
   A[Your computer] -- Only via Hopkins/VPN --> D((Web Portal))
   A[Your computer] --> C([Transfer node])
   B([Login nodes]) --> E[Compute node1]
   D((Web Portal)) --> E[Compute node1]
   B([Login nodes]) --> F[Compute node2]
   D((Web Portal)) --> F[Compute node2]
   B([Login nodes]) --> G[Compute node3]
   D((Web Portal)) --> G[Compute node3]
   C([Transfer node]) <--> H[(storage servers)]
```
-->

## Public-facing: Login and Transfer 
The login and transfer nodes are accessible to the wider Internet. 
For security reasons, the web portal is only available to computers on Hopkins networks. If you are not on a Hopkins campus, that means that you need to use the VPN to be able to see that node.

* **login**: Normally jhpce01.jhsph.edu and jhpce02.jhsph.edu
* **transfer**: {==jhpce-transfer01.jhsph.edu==}
* **web portal**: {==[jhpce-app02.jhsph.edu](https://jhpce-app02.jhsph.edu)==}


## SSH Is The Primary Method
Access to the JHPCE cluster requires the use of SSH.

SSH stands for Secure SHell. SSH is actually a set of internet standard protocols. Programs implementing these protocols include both command line interface (CLI) tools and those with graphic user interfaces (GUI).  They all enable you to make secure, encrypted connections from one computer to the next.

[This document](ssh.md) provides more information on SSH.

## Multi-factor authentication (MFA)
There are two basic "factors" required to log into a computer, whether your laptop or a remote UNIX cluster login node -- your username and a password. {==JHPCE requires the use of an additional factor==}, either a [one-time password](ssh.md#one-time-passwords) (OTP) six digit code, or the use of [SSH key pairs](ssh.md#ssh-keys). 

### One Time Passwords
When you SSH into JHPCE, you will be prompted for a “Verification Code:” This is your cue to enter in a one-time password six digit code.

Programs like `Google Authenticator` and `Micrsoft Authenticator` generate one-time password codes (OTP). These are only good for a single use, whether you successfully log in or not. Typically they are used to generate a _stream_ of time-based OTPs, or TOTPs. These are only good for one minute, adding another layer of difficulty for someone trying to impersonate you.

These programs are usually used on smartphones, but there are programs available to create them on laptops and desktops. The key with using ANY OTP program is to get it from a trusted source. We will default to mentioning the `Google Authenticator`.

After you log into JHPCE for the first time, you should immediately configure your OTP program using a "secret" accessible to you on the cluster via the `auth_util` program. Instructions for doing that are found in the [orientation documents](https://docs.google.com/presentation/d/1elMSTUdKws7FLVFK7vVV_AErA4brNSPX/pub){:target="_blank"}.


## X11/The X Window System

The [X Window System](https://en.wikipedia.org/wiki/X_Window_System) (aka X11, aka X) is the primary GUI system for UNIX computers. X allows a program (an X "client") running on a UNIX computer in the cluster to be displayed on a remote computer (running an X "server"[^2]) over the network. To use it, your local computer needs to have X server software installed on it.

SSH provides support for tunnelling X11 over an encrypted connection. You may need to tell SSH that you want that service, by, for example, adding the -X flag to an ssh command in a macOS Terminal.

[^2]:Note that X reverses the normal conception of client/server operation, which is that the remote computer is the "server". In X11, the "server" is the program receiving the keyboard and mouse inputs and displaying the output of remote "client" programs.

- macOS users need to install [XQuartz](https://en.wikipedia.org/wiki/XQuartz) from [xquartz.org](https://www.xquartz.org).
- Windows users need to use a program like MobaXterm (highly recommended) or [Cygwin](https://en.wikipedia.org/wiki/Cygwin). We have a page describing installing and using [MobaXterm](mobaxterm.md).
- Linux laptop or desktop users are already using X as their windowing system.

For more information, see [our X11 document](x11.md).

<!-- 20251215 - are we going to offer a way to change password etc?
## Web Portal
We have a web server named [jhpce-app02.jhsph.edu](https://jhpce-app02.jhsph.edu) configured to offer a growing number of services. Click on the links to learn more.

* Reset your password or generate a OTP ([learn more](../portal/web-reset.md))
* Run applications on the cluster (RStudio, JupyterLab, VS Code) ([learn more](../portal/web-apps.md))
* Inspect a catalog of research databases (under development) ([learn more](../portal/datacatalog.md))
-->

## SAFE and SAFER Desktops
The SAFE Desktop was created by the School of Medicine to enable secure
computing that involved sensitive (PHI, PII) information. The [Research
IT](https://researchit.jhu.edu/about-rit/) group has been creating a more secure
facility named the SAFER Desktop.

Both are virtual Windows desktops equipped with applications for researchers,
such as SAS and STATA, as well as productivity and utility tools. They include
a certain amount secure data storage for sensitive (PHI, PII) information,
which can be shared by others using the same facility.

Both {==require having JHED accounts and filling out a form.} It can be several
days before the request is fulfilled, so please plan ahead.

SAFE (the [Secure Analytic Framework
Environment](https://researchit.jhu.edu/safer/) is free (students need faculty
sponsorship), and comes with 100GB of disk space.

SAFER (the [Secure Analytic Framework Environment for
Research](https://researchit.jhu.edu/safer/)) is provisioned using commercial
cloud providers. They offer several different models with varying capabilities.
The lowest model is free.

## File Transfers

We have a transfer server {==jhpce-transfer01.jhsph.edu==} for file transfers into and out of the cluster. It is connected by a 40G Ethernet link to Hopkins networks.
This computer also offers a Globus Endpoint service (described [here](globus.md)) for transfers from personal computers and other institutions.

Transferring data _into or out of the cluster_ is documented [here](../access/file-transfer.md).

The login nodes SHOULD NOT be used for file transfers into and out of the cluster beyond extremely trivial cases. Their connections are four times slower and they are relied upon by all of your peers.

Please use compute nodes for transfers _WITHIN the cluster_. For example, copying significant volumes of files from one file system to another, such as `/dcs05/a-place/` to `/dcs07/somewhere-else/`. Information about doing that can be found [here](../files/copying-files.md)

