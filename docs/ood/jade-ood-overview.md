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
    To use [JADE's OOD server](https://jade-ondemand01.jhsph.edu), you need to use a dedicated OOD One Time Password (OTP). This is in addition to the OTP that you use when logging into the `jade01.jhsph.edu` server using SSH. You need to maintain access to two OTP "accounts" to be able to log in via SSH and OOD. ==When logging in with SSH and OOD, you use the same username & password, but the OTP's are different.==

The JADE cluster's security model requires the use of a multi-factor credential during login. When you use SSH, you need to provide a username, password and a `One Time Password (OTP)` (technically it is a Time-based One Time Password (TOTP)). The OTP is also called a `verification code`.   Users configure applications like Google or Microsoft Authenticator using a user-specific "secret" 

On JADE, we use a software package called Keycloak to handle the MFA credential component. 

Instructions will follow on the web site about working with OTP changes.
