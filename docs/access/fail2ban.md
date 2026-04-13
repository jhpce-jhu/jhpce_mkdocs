---
tags:
  - jeffrey
---


# Fail2ban - Protecting Our Clusters From Hackers

Example error message:

"ssh: connect to host jhpce01.jhsph.edu port 22: Connection refused"

## What
Our public-facing servers use a software package called `fail2ban` which monitors incoming connection requests and temporarily bans IP addresses if they fail to log in successfully too many times within a certain time window.

## Why

We have configured the fail2ban settings to allow real users to fail a bit but fairly quickly lock out in-person or automated hackers.

## Ban details

Each set of three failed ssh attempts gets your IP address banned from contacting that server. Each server keeps its own connection database and imposes its own bans.

Ban time progression:

- The first three bans are for 10min each,
- next two are for 20min each,
- then the sixth is 5hr.

If you hit eight short-term bans within seven days your IP address is banned for 4 weeks. We expect that real users will reach out to us fairly early in that sequence.

Remember, each short term ban requires three failed login attempts, so in order to be banned for 5 hours requires five times three equals 15 failures.

## Troubleshooting the causes of login failures

Please consider what is causing you to fail repeatedly enough to be banned.

### Multi-factor authentication

Our servers require multi-factor authentication (MFA).

Some SSH/SFTP client programs, particularly GUI ones, may require you to check a preference box to let that app know that it needs to be prompting you for a One-Time Password (OTP), which we call verification codes.

If you are copy-and-pasting verification codes, consider entering them by hand. This prevents you from inadvertently copying a trailing space character.

Users of our main JHPCE cluster can request a password reset or a scratch verification code using our web portal https://jhpce-app02.jhsph.edu. You must have a current JHEDID and log in from a computer on a Hopkins network.

### Memorized passwords
We recommend that users do not tell their SSH/SFTP client programs (e.g. MobaXterm or CyberDuck or FileZilla or VS Code) to remember their passwords. Primarily that is because it leads users to forgetting what their passwords are. Secondarily, the default settings for some of these programs is to retry connection attempts, perhaps without your even asking for a fresh connection. (For example an SFTP client might want to reconnect you and resume a transfer if the network drops.)

We have [configuration instructions](file-transfer.md) for MobaXterm (Windows) and Cyberduck (macOS).

## Dealing with bans

These bans are of your IP address, not your account. 

Our main JHPCE cluster has two, equivalent, login servers, each of which maintains its own database of failed logins. So, if `jhpce01.jhsph.edu` has blocked you, you can try `jhpce03.jhsph.edu`.

Our JADE cluster has only one login server, `jade01.jhsph.edu`.

Both clusters have only one dedicated file-transfer server.

In a pinch, if you can use a new IP address to originate your login attempt you can do that. Perhaps log into another server somewhere else, then connect to JHPCE or JADE.








