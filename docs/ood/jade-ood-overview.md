---
tags:
  - ood
  - jade
  - in-progress
  - topic-overview
---

# Open OnDemand (OOD) Overview - JADE Cluster

This document will serve as a launch point for instructions on how to use [OOD](https://www.openondemand.org/) on JADE. That server can be reached at [https://jade-ondemand01.jhsph.edu](https://jade-ondemand01.jhsph.edu).

This document will not contain all of the information about OOD, only an overview.

## About OOD

Open OnDemand provides remote access to a cluster like JADE via web browsers rather than SSH applications. No client software needs to be installed on users' computers. Work can be done using a Graphic User Interface (GUI) as well as a terminal for Command Line Interface (CLI) work.

## JADE-specific details

### An additional MFA code just for JADE OOD

!!! Warning "Two different OTP for JADE"
    To use [JADE's OOD server](https://jade-ondemand01.jhsph.edu), you need to use a dedicated One Time Password (OTP).  This is different from, and _in addition to,_ the OTP that you use when logging into the `jade01.jhsph.edu` server using SSH. In other words, to be able to log in via both SSH and the Open OnDemand web interface, you will need to maintain access to two "OTP accounts".  ==You use the same username & password no matter which interface you use, but the OTP's are different.==

The JADE cluster's security model requires the use of a multi-factor credential during login. When you use SSH, you need to provide a username, password and a `One Time Password (OTP)` (technically it is a _Time-based_ One Time Password (TOTP)). The OTP is also called a `verification code`.   Users configure applications like Google or Microsoft Authenticator using a user-specific "secret" which is either a QR code image or a "secret" string of text characters.

On JADE, we use a software package called [Keycloak](https://www.keycloak.org/) to handle the MFA credential login process. 

Instructions will follow on this web site about working with Keycloak to do things like generating a new OTP.

### File transfer capabilities are restricted

By default, Open OnDemand offers file transfer facilities both natively and through options in applications you launch via OOD, such as Jupyter.

{==The Open OnDemand server configuration has been modified to disable file transfer in a number of places.==}

This was done to meet essential security requirements (i.e. NIST standard 800-171) which are the whole reason why JADE was created. We completely understand the inconvenience this creates.

The normal way JADE users push data into or pull files out of JADE is by using SFTP on an external computer to connect to unique TCP/IP networking port numbers. Our master web page about JADE data transfers is [here](../jade/access/transfer-overview.md).

One of the primary JADE user communities, CMS, has Data Usage Agreement requirements which require a team of moderators to review each file before it is able to be pulled out of the cluster.

Some specific disabled functionalities:

- OOD's File Browser - Upload/download buttons were disabled.
- Rstudio - A flag was changed in the RStudio configuration file.
- VS Code - The codeserver configuration includes `--disabled-file-download`
- Jupyter - A setting was changed in a JSON file.

It is impossible to prevent all data transfers from occurring via all of the applications which can be launched using OOD. {==CMS users must understand that the existence of that capability does not relieve them of the legal requirement to go through the HARP-approved data moderation process.==}
