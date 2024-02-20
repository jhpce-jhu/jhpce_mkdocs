---
tags:
  - in-progress
  - topic-overview
---

# Accessing the Cluster: Overview

## Cluster structure overview
Basically the cluster consists of some public-facing hosts and then resources isolated from other networks. Insert a simplistic diagram or an updated one of the one in the Orientation. Help people visualize.

## Public-facing: Login and Transfer 
What is visible to the Internet?
Mention that some interfaces require being on Hopkins networks (is this only the C-SUB's login node?)

(Whether here or in some other more-specific document: Do we want to say that we recommend being on a Hopkins network because of any factors? Better speed if you're actually on a Hopkins network? Lower speed if you use VPN? Anything?)

## SSH Is The Primary Method
Access to the JHPCE cluster requires the use of SSH.

SSH stands for Secure SHell. SSH is actually a set of internet standard protocols. Programs implementing these protocols include both command line interface (CLI) tools and those with graphic user interfaces (GUI).  They all enable you to make secure, encrypted connections from one computer to the next.

[This document](ssh.md) provides more information on SSH.

## Multi-factor authentication (MFA)
There are two basic factors required to log into a computer, whether your laptop or a remote UNIX cluster login node -- your username and a password. JHPCE requires the use of an additional factor, either a [one-time password](ssh.md#one-time-passwords) (OTP) six digit code, or the use of [SSH key pairs](ssh.md#ssh-keys). 

### One Time Passwords
When you SSH into JHPCE, you will be prompted for a “Verification Code:” This is your cue to enter in a one-time password six digit code.

Programs like `Google Authenticator` and `Micrsoft Authenticator` generate one-time password codes (OTP). These are only good for a single use, whether you successfully log in or not. Typically they are used to generate a _stream_ of time-based OTPs, or TOTPs. These are only good for one minute, adding another layer of difficulty for someone trying to impersonate you.

These programs are usually used on smartphones, but there are programs available to create them on laptops and desktops. The key with using ANY OTP program is to get it from a trusted source. We will default to mentioning the `Google Authenticator`.

After you log into JHPCE for the first time, you should immediately configure your OTP program using a "secret" accessible to you on the cluster via the `auth_util` program. Instructions for doing that are found in the [Orientation document](../orient/images/latest-orient.pdf).

## Web Portal
Describe capabilities of web portal.
Links to the apps page(s) about it.

## SAFE Desktop
Do we mention this as an access channel?

## File Transfers
Brief description of our transfer node and its capabilities. Admonition to not use login node for transfers (either in or out or within the cluster)

Point to some overview doc about this topic, which is going to have several documents in total, not one giant one, i.e.

* overview doc
* globus
* windows-specific? recommend a tool
* Internal file transfers - this deserves a document. Perhaps under the Managing Files header. Do's, don'ts, sample batch job, good rsync args to use, etc.


