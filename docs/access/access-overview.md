---
tags:
  - topic-overview
title: "Access Overview"
---

# Accessing the Cluster: Overview

## Cluster structure overview
The cluster consists of some public-facing hosts with the remaining computers "behind them" on private networks. An incomplete and very oversimplified approximation:

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
Ways in which this diagram is oversimplified include

- the diagram represents our main cluster. Our secure cluster, JADE (JHPCE Advanced Data Enclave), cannot be accessed at all unless you are connecting from a Hopkins network
- users don't log into compute nodes directly but use SLURM to launch batch jobs or open interactive sessions
- the compute nodes also conduct NFS (Network File Service) with the storage servers
- same with the transfer server and the storage servers

## Public-facing: Login and Transfer 
The login and transfer nodes are accessible to the wider Internet. 
For security reasons, the web portal is only available to computers on Hopkins networks. If you are not on a Hopkins campus, that means that you need to use the VPN to be able to see that node.

| Purpose | JHPCE Cluster | JADE Cluster |
| ------- | ------------- | ------------ |
| **login** | jhpce01.jhsph.edu & jhpce03.jhsph.edu | jade01.jhsph.edu |
| **transfer** | jhpce-transfer01.jhsph.edu | jade-transfer01.jhsph.edu |
| **web portal** | [jhpce-app02.jhsph.edu](https://jhpce-app02.jhsph.edu) | [jade-ondemand01.jhsph.edu](https://jade-ondemand01.jhsph.edu) |


## SSH Is The Primary Method
Access to the JHPCE cluster requires the use of SSH.

SSH stands for Secure SHell. SSH is actually a **set** of internet standard protocols. Programs implementing these protocols include both command line interface (CLI) tools and those with graphic user interfaces (GUI).  They all enable you to make secure, encrypted connections from one computer to the next.

[This document](ssh.md) provides more information on SSH.

## Multi-factor authentication (MFA)
Our clusters are configured to require MFA.

There are two basic "factors" required to log into a computer, whether your laptop or a remote UNIX cluster login node -- your username and a password. {==JHPCE requires the use of an additional factor==}, either a [one-time password](ssh.md#one-time-passwords) (OTP) six digit code, or the use of [SSH key pairs](ssh.md#ssh-keys). 

### One Time Passwords
When you SSH into JHPCE, you will be prompted for a “Verification Code:” This is your cue to enter in a one-time password six digit code.

Programs like `Google Authenticator` and `Micrsoft Authenticator` generate one-time password codes (OTP). {==These are only good for a single use, whether you successfully log in or not.==} Typically they are used to generate a _stream_ of time-based OTPs, or TOTPs. These are only good for one minute, adding another layer of difficulty for someone trying to impersonate you.

These programs are usually used on smartphones, but there are programs available to create them on laptops and desktops. The key with using ANY OTP program is to get it from a trusted source. We will default to mentioning the `Google Authenticator`.

After you log into JHPCE for the first time, you should immediately configure your OTP program using a "secret" accessible to you on the cluster via the `auth_util` program. Instructions for doing that are found in the [orientation documents](https://docs.google.com/presentation/d/1elMSTUdKws7FLVFK7vVV_AErA4brNSPX/pub){:target="_blank"}.

## Fail2ban - Protecting Against Hackers

We use a package called fail2ban on our public-facing servers to watch for IP addresses which are trying and failing to connect. After specific numbers of failures within various time windows, IP addresses are banned - again for various lengths of time.

For more information, please see our [fail2ban](fail2ban.md) document.

## X11/The X Window System

The [X Window System](https://en.wikipedia.org/wiki/X_Window_System) (aka X11, aka X) is the primary GUI system for UNIX computers. X allows a program (an X "client") running on a UNIX computer in the cluster to be displayed on a remote computer (running an X "server"[^2]) over the network. To use it, your local computer needs to have X server software installed on it.

SSH provides support for tunnelling X11 over an encrypted connection. You may need to tell SSH that you want that service, by, for example, adding the -X flag to an ssh command in a macOS Terminal.

[^2]:Note that X reverses the normal conception of client/server operation, which is that the remote computer is the "server". In X11, the "server" is the program receiving the keyboard and mouse inputs and displaying the output of remote "client" programs.

- macOS users need to install [XQuartz](https://en.wikipedia.org/wiki/XQuartz) from [xquartz.org](https://www.xquartz.org).
- Windows users need to use a program like MobaXterm (highly recommended) or [Cygwin](https://en.wikipedia.org/wiki/Cygwin). We have a page describing installing and using [MobaXterm](mobaxterm.md).
- Linux laptop or desktop users are already using X as their windowing system.

For more information, see [our X11 document](x11.md).

## Web Portals

Access to our clusters will be changing in 2026. The main source of information will be the (now-sparse) page about [web services](web.md).

We have a web server named [jhpce-app02.jhsph.edu](https://jhpce-app02.jhsph.edu) configured to offer a number of services. Click on the links to learn more.

* Reset your password or generate a OTP ([learn more](../portal/web-reset.md)) (**{==requires a valid JHEDID account==}**)
* Run applications on the cluster (RStudio, JupyterLab, VS Code) ([learn more](../portal/web-apps.md))
* Inspect a catalog of research databases (under development) ([learn more](../portal/datacatalog.md))

## SAFE & SAFER Desktops
Hopkins central IT offers two services named SAFE and SAFER Desktop. These are virtual Windows computers which can be used by themselves or to access JHPCE clusters such as JADE.

Members of the dbGaP community **MUST** use one of these two services to access JADE in order to meet the requirements of the NIST 800-171 security standard.

More information about them including links to application forms can be found in [our page](../access/safe-safer-desktops.md).

## File Transfers

We have a transfer server {==jhpce-transfer01.jhsph.edu==} for file transfers into and out of the cluster. It is connected by a 40G Ethernet link to Hopkins networks.
This computer also offers a Globus Endpoint service (described [here](globus.md)) for transfers from personal computers and other institutions.

Transferring data _into or out of the cluster_ is documented [here](../access/file-transfer.md).

The login nodes SHOULD NOT be used for file transfers into and out of the cluster beyond extremely trivial cases. Their connections are four times slower and they are relied upon by all of your peers.

Please use compute nodes for transfers _WITHIN the cluster_. For example, copying significant volumes of files from one file system to another, such as `/dcs05/a-place/` to `/dcs07/somewhere-else/`. Information about doing that can be found [here](../files/copying-files.md)

