
# SSH - Key Information
   
## SSH Basics

Access to the JHPCE cluster requires the use of SSH. Over time, the ability to launch applications via a web interface will increase, but for the foreseeable future you will need to be able to SSH in and then use command line UNIX skills to do your work.

SSH stands for Secure SHell. SSH is actually a set of internet standard protocols. Programs implementing these protocols include both command line interface (CLI) tools and those with graphic user interfaces (GUI).  They all enable you to make secure, encrypted connections from one computer to the next.  In our case, ssh allows you to create an encrypted connection between you local system and the JHPCE login nodes (jhpce01.jhsph.edu and jhpce03.jhsph.edu), so that you can run a Unix Shell on the JHPCE cluster.

Depending on the kind of operating system your computer uses, you may or may not need to install SSH software. Apple Macs come with CLI SSH tools pre-installed. You use them by entering commands in the Terminal app.  You can install GUI apps from various vendors, but we will only discuss the CLI tools except for some file transfer GUI programs.  On a Windows system you will need to install an ssh client.  We recommend the excellent GUI program MobaXterm. [Here is a document](mobaxterm.md) describing how to use it. There are other programs, such as the [PuTTY](https://en.wikipedia.org/wiki/PuTTY) family of tools and [WinSCP](https://en.wikipedia.org/wiki/WinSCP).

By default, we use "2 Factor Authentication" in order to authenticate you access when you ssh into the JHPCE cluster.  This means that we will ask for 2 pieces of infomation instead of just 1.  Secifically, you will be asked for:

1) Your Password. Your password will be a secure secret that only you know and is hard for others to guess.
2) A Verification Code.  Your Verification Code will be a one-time-use code from an App like Google Authenticator.

As an aside, there are commonly 3 factors that can be used when authenticating in the Cybersecurity world. These are, Something you Know, Something you Have, and Something you Are.  The 2 factors we're using on JHPCE are "Something you Know" (a password), and "Somthing you Have" (your one-time-use code from Google Authenticator). An example of methods that use "Something you Are" would be a fingerprint scanner or the Apple's Face ID.

You can also allow the use of SSH keys to help smooth the login process with a passwordless process.  Please see [the section below](https://jhpce.jhu.edu/access/ssh/#ssh-keys) for more details on SSH keys.

Here is what a normal ssh connection looks like to login to the jhcpe01.jhsph.edu login node as user "bsmith", coming from his Mac named "Bobs-Mac".

<pre lang="console"><code>
[bobsmith@Bobs-Mac ~] $ <b>ssh bsmith@jhpce01.jhsph.edu </b>
(bsmith@jhpce01.jhsph.edu) Password: <i>Bob types his JHPCE password</i>
(bsmith@jhpce01.jhsph.edu) Verification code: <i>Bob types the 6-digit code from Google Authenticator</i>
Last failed login: Mon Apr  7 09:42:08 EDT 2025 from 174.172.134.59 on ssh:notty
Last login: Mon Apr  7 09:23:33 2025 from 174.172.134.59
🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸
Use of this system constitutes agreement to adhere to all
applicable JHU and JHSPH network and computer use policies.
🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸 🌸

Need Help? bitsupport@lists.jh.edu for SLURM, cluster, storage & login issues
           bithelp@lists.jh.edu for application issues: R/SAS/python/modules...

The SLURM shared partition is currently at 85% core usage and 70% RAM usage.
--------------------------------------------------------------------
DISK: Compression is enabled in our ZFS file systems like /users & /dcs05
DISK: You don't need to compress your files unless you want to use _very_
DISK: high compression levels or transfer them outside of the cluster.
--------------------------------------------------------------------
--------------------------------------
     Your Home Directory Usage        
Username     Space Used         Quota     
bsmith       21G                100G      
--------------------------------------

[bsmith@jhpce01 ~]$ 
</code></pre>

!!! Note
    Please note that when typing the "Password" and "Verification Code", the cursor does not move.  This is a security mechanism to prevent someone from watching what you are typing.  The computer is seeing what you are typing even if it isn't being displayed back.

If you have entered your credentials correctly, you will see a login banner for the JHPCE cluster, and you will be sitting at a shell prompt on the login node.  However, if you are prompted for “Password:” again, you likely mistyped either your password or Verification Code, and you should wait until your Google Authenticator code changes (it changes every 30 seconds) and you will need to try logging in again.

Here are some things to check if you are unable to ssh into the JHPCE cluster:
- Please make sure that you are using the password for the JHPCE cluster, and not your some other password, like you JHED or laptop password.
- Please make sure you are using your JHPCE login ID. This is different from your JHED ID.
- Please make sure you are using the Google Authenticator entry for the JHPCE cluster, and not some other system.

If you have had several unsuccessful login attempts, your local system will be blocked by the login node.  Further attempts to ssh into the login node will likely give you a “Connection Refused” message, you will need to wait 10 minutes before trying to login again.  It you are certain you are using all of the right information for your login, and don't want to wait 10 minutes, you can try our second login node, jhpce03.jhsph.edu.

If you continue to have login issues, please contact us at bitsupport@lists.jhu.edu.



## SSH Keys

#### SSH Keys Quickstart:
For Windows users using MobaXterm, please see this [guide on our site in the Mobaxterm section](https://jhpce.jhu.edu/access/mobaxterm/#optional-setting-up-ssh-keys-in-mobaxterm)

For Mac or Windows users, you can use the following steps.

1) Generate your private and public keys on your local system, run "ssh-keygen -t ecdsa".  When asked "Enter file in which to save the key", you should use the default answer. When asked to enter a passphrase, please choose a secure passphrase/password to securely store your keys.

<pre lang="console"><code>
[bobsmith@Bobs-Mac ~] $ <b>ssh-keygen -t ecdsa </b>
Generating public/private ecdsa key pair.
Enter file in which to save the key (/Users/BobSmith/.ssh/id_ecdsa): 
Enter passphrase (empty for no passphrase): <i>Passphrase Entered</i>
Enter same passphrase again: <i>Passphrase Entered</i>
Your identification has been saved in /Users/BobSmith/.ssh/id_ecdsa
Your public key has been saved in /Users/BobSmith/.ssh/id_ecdsa.pub
The key fingerprint is:
SHA256:uyy1HdCMcwNMSHLk2cKsc0Tn bobsmith@Bobs-Mac.local
The key's randomart image is:
+---[ECDSA 256]---+
| *AB.  .o ..     |
. . .
|    o .   .  .   |
|     .   A.o.    |
+----[SHA256]-----+
</code></pre>
2) Run "ssh-agent" and "ssh-add" to enable ssh-agent on your local system.  This will enble you to use your keys without having to use your Passphrase every time you use them.  You will need to run these 2 commands each time you reboot your local system.
<pre lang="console"><code>
[bobsmith@Bobs-Mac ~] $ <b>ssh-agent</b>
SSH_AUTH_SOCK=/var/folders/xb/8jxv_1495w7f7ym9w89ksg8m0000gn/T//ssh-IAaQtZb85RG5/agent.63154; export SSH_AUTH_SOCK;
SSH_AGENT_PID=63155; export SSH_AGENT_PID;
echo Agent pid 63155;

[bobsmith@Bobs-Mac ~] $ <b>ssh-add </b>
Enter passphrase for /Users/bobsmith/.ssh/id_ecdsa: ***Passphrase Entered***
Identity added: /Users/bobsmith/.ssh/id_ecdsa (bobsmith@Bobs-Mac.local)
</code></pre>
3) Copy your ssh public key up to the JHPCE cluster.  Please be sure to replace the "bsmith" in the example below with your JHPCE login ID.
<pre lang="console"><code>
[bobsmith@Bobs-Mac ~] $ <b>cat ~/.ssh/id_ecdsa.pub | ssh bsmith@jhpce01.jhsph.edu 'cat >> ~/.ssh/authorized_keys'</b>
(bsmith@jhpce01.jhsph.edu) Password: <i>Enter your JHPCE Password</i>
(bsmith@jhpce01.jhsph.edu) Verification code: <i>Enter your Google Authenticator Code</i>
Bobs-Mac$
</code></pre>
4) Now you should be able to login without having to use your JHPCE password and verification code.
<pre lang="console"><code>
[bobsmith@Bobs-Mac ~] $ <b>ssh bsmith@jhpce01.jhsph.edu </b>
Last failed login: Mon Apr  7 09:42:08 EDT 2025 from 174.172.134.59 on ssh:notty
Last login: Mon Apr  7 09:23:33 2025 from 174.172.134.59
Use of this system constitutes agreement to adhere to all
applicable JHU and JHSPH network and computer use policies.

. . .

[bsmith@jhpce01 ~]$ 
</code></pre>


#### SSH Keys, more info:
SSH programs make use of something called [public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography). Basically secure communications can be created by splitting a secret into to parts, and placing one part on each end of a link. This can be extended to an optional pair of files you can generate and distribute such that one is located on the JHPCE cluster and the other is on your computer. Or smartphone!

This key pair of files is generated once, and protected with a passphrase. You place a copy of the "public" key file on JHPCE in a particular place with specific permissions. You keep a copy of the "private" key file on your personal device(s). Once you prove to a program (ssh-agent) running on your device that you know the passphrase to your private key file, it will thereafter provide your private key when you run an SSH command.

Once configured properly, you can use SSH keys instead of your JHPCE password.

[SSH Keys described by another cluster](https://hpc-docs.cubi.bihealth.org/connecting/ssh-basics/#ssh-keys)

This is still a means for using 2 Factor Authentication.  The key files themselves are "Something you Have" and these are protected by "Something you Know" in the form of the SSH passphrase, as well as the authentication mechanism used to protect your local system (password/fingerprint scanner)

## Just yanked in here from our Knowledgebase document

!!! Note "Authoring Note"
    This material needs to be placed in the right places.




### Unix-based machines (linux and mac osx)

SSH supports different kinds of identity key encryption.  Various algorithms have been developed over the years. As computing power increases and algorithm flaws are discovered, the recommended one to use changes. In the past "rsa" was the default.  That is no longer recommended in 2024. We here show "ecdsa" but you might want to instead use "ed25519". Just be consistent in your use of the string where it shows up in commands or file names.

+ First, on your local laptop/desktop, open a terminal and `cd` into your home directory  and invoke the ssh key generator:

```
ssh-keygen -t ecdsa
```

+ You will be prompted for a passphrase.  For security reasons, we require that you use a passphrase to protect your key.  This prevents someone who gets access to your private key file from being able to use it!!!!! For the other questions, you can select the default values.
+ The key generator will put two files in the .ssh subdirectory of your home directory, typically  `~/.ssh/id_ecdsa` and  `~/.ssh/id_ecdsa.pub`. 
+ *You should only ever have to run the ssh key generator once on your local host*.  If you have already configured passwordless login and you run the key generator a second time, it will overwrite your previous public and private key files. This will break all password-less logins that you set up with your previous keys.
+ Next you want to copy your public key file to the remote host and append it to the authorized_keys file in the `.ssh` subdirectory of your home directory on the remote host. 
  + If there is no `~/.ssh` directory in the remote host, you will need to login to the remote host and create one. Note, attempting to connect to another host, like `ssh github.com`, will create one if it isn't there already. 
+ You can perform the copy and append operations in one line as follows;

```
cat ~/.ssh/id_ecdsa.pub | ssh <your_userid>@jhpce01.jhsph.edu 'cat >> ~/.ssh/authorized_keys'
```
+ Where you replace `<your_userid>` with your JHPCE userid and where you enter your JHPCE password when you are prompted for it by ssh.
+ To test that everything is working you should be able to log into the remote host from your local host with the following command`ssh <your_userid>@jhpce01.jhsph.edu`.
+ When you start ssh, you will be prompted for the passphrase that you used to protect your key.  To avoid having to enter your passphrase every time you use ssh, you can use the `ssh-agent` program.  To use `ssh-agent`, run

```
ssh-add
ssh-agent
```
The ssh-agent will remain active for as long as your desktop or laptop is up and running.  If you reboot your desktop/laptop, you will need to rerun the ssh-add and ssh-agent commands.

#### Loging into nodes

!!! Warning "Authoring Note"
    (((This isn't true if you use `srun`. Is it even true in JHPCE 3.0 anyway Also, do we want to be suggesting that people log into compute nodes?)))

Logging into a cluster node from a login node requires keypairs. 
 If
your private key file is in `.ssh/` then it should work, since the
public key file is in your `authorized_keys` file. If you do not want
the same private key file to be used to log into nodes as the one used
to log into the login node, then repeat create a new public private
key / public key pair on the cluster.  Append the public key to
your`authorized_key` file. Note, when appending, add the public key,
do not overwrite the existing file.

Many users set up an alias for the ssh command so they don’t have to type as much to log into the remote host.  You can do this by adding the following line to your `~/.bashrc`, `alias hpc='ssh -X <your_userid>@jhpce01.jhsph.edu'`.

### Windows machines 
If your desktop/laptop runs Microsoft Windows then you first need to
install MobaXterm on your windows machine. If you are
using MobaXterm, please use the steps at the bottom of our [MobaXterm
configuration page](mobaxterm.md).

## Permissions on SSH Files
SSH is **_very_** strict about the permissions found on your ssh-related files on **both ends** of a connection. These files are found on JHPCE in your home directory inside the directory `.ssh`  Because this directory's name begins with a period, it is not listed when you use the `ls` program -- you have to use `ls -a` or `ls -ld ~/.ssh` to see it.

The primary symptom of there being a file permissions problem is that ssh is still asking for a password when you think it should not. Sometimes you get a helpful error message `Bad owner or permissions on <a specific ssh-related file>`

These rules are normally found to be broken on the remote side of a connection, but the permissions on your computer also matter. Different SSH programs may keep ssh-related files in different places, especially if they are on Windows computers.

ALL OF THESE FILES NEED TO BE OWNED BY YOUR ACCOUNT.

This table shows you two forms of the chmod command arguments needed to force permissions to be acceptable by SSH. The most convenient notation used by chmod is an octal (base-8) number. The most readable notation is a comma-separated combination of letters.

These two commands are equivalent:

* `chmod 700 $HOME/.ssh`
* `chmod u+rwx,g-rwx,o-rwx $HOME/.ssh`


|File/Directory|Octal|Human Readable|Note|
|-------|-------------|-------------|----|
|$HOME|755 or tighter|g-w,o-w|Not writable by group or other|
|$HOME/.ssh|700|u+rwx,g-rwx,o-rwx|No access by group or other|
|$HOME/.ssh/authorized_keys|600|u+rw,g-rwx,o-rwx|Authorized keys file|
|$HOME/.ssh/config|600|u+rw,g-rwx,o-rwx|Config file|
|$HOME/.ssh/id_*|600|u+rw,g-rwx,o-rwx|Private key files|
|$HOME/.ssh/id_*.pub|644|u+rw,g+r,g-wx,o+r,o-wx|Public key files|



## SSH and X11

See our document on [X11](x11.md) for instructions on making ssh work to support X11 displays.

## Mac-specific Configuration

!!! Note
    You do not **need** to do ANY configuration for things to work, including running X11. All you need to do is to install the XQuartz package to support X11 window display. (The following material assumes you have installed it, and rebooted afterwards.)

You can choose to put configuration information into a file in your home directory to make your life easier by, for example, specifying the username of some remote computer if it differs from your local username.  If you do that, then you no longer have to provide that information for every ssh command. {==However, you then need to remember that you have made choices in that file when something doesn't work the way you expect.==}

### Config File Overview

SSH is a client-server program. SSH client programs like `ssh` connect over networks to SSH server programs (usually named `sshd`).  You don't have to worry about `sshd` configuration unless you are managing a computer that accepts incoming SSH connections.  macOS and Linux computers can run sshd if you enable that service (by checking the box "Remote Login" in macOS' Sharing pane of System Preferences).

Both client and server have configuration files. There are _system-wide_ ones for each, typically located in /etc/ssh/, and _per-user_ ones, typically located in $HOME/.ssh, e.g. /Users/yourusername/.ssh on a Mac). A tilde symbol used in a shell like bash is a shortcut to "my home directory" so $HOME/.ssh and ~/.ssh should be equivalent ways of specifying the same directory. You can read about user SSH configuration files, the order in which they are read, and the meaning of the options, with the command `man ssh_config`. 

{==Do not add anything that you do not understand !! ==}

### Changing Your Config File
You do not need ANY per-user config file for things to work, including running X11 (at least not when tested in macOS 12.6.3).

: The only line you *might* need in your ~/.ssh/config file is this:
: `XAuthLocation /opt/X11/bin/xauth`
: if you are told that xauth is missing. This location has been the standard used by XQuartz -- it might change in the future. The file specified needs to be executable and exist at that location. You can test it by running it with the arguemnt `/opt/X11/bin/xauth list`

**In your ~/.ssh/config file you may find some of the following options useful.** 

The basic structure of this file is:

1. some general settings
2. some (optional) host-specific settings, in stanzas
3. some (optional) settings meant to apply to all hosts not previously specified, in a stanza which begins with a line `Host *`

#### General Settings

```Shell title="Useful general macOS ~/.ssh/config file settings"
EnableEscapeCommandline yes # Needed for recent macOS if you want to issue an SSH escape,
#.      to create or close a tunnel for example
ForwardX11 yes        # Equiv to "-X" arg
ForwardX11Trusted yes # Equiv to "-Y" arg
XAuthLocation /opt/X11/bin/xauth  # only include if needed
# These are for users of public keys
UseKeychain yes       # look for passphrases for public keys in macOS keychain
AddKeysToAgent yes    # store passphrases for public keys in macOS keychain
# These keys are tried in order when logging in, and are loaded by your SSH agent
IdentityFile ~/.ssh/id_ecdsa
IdentityFile ~/.ssh/id_rsa
ForwardAgent yes      # Equiv to the "-A" arg. For ssh'ing from 1 to 2 to 3. 
```
```Shell title="Keeping X11 forwarding enabled"
ForwardX11Timeout 0 # disables the timeout, which is by default 20mins
                    # you can also set it to a time value like "336h" (2 weeks)
```

```Shell title="Keeping connections alive - method one"
ServerAliveInterval 15
ServerAliveCountMax 30
# These values mean that your client ssh program will send a message to the server
# every 15 seconds, and not decide that a remote server is unresponsive until
# (15*30)=450 seconds
```

```Shell title="Keeping connections alive - method two"
TCPKeepAlive no
# TCPKeepAlive defaults to YES, so you have to intentionally disable it.
# The author of this web page chose to disable TCPKeepAlive at one time (and then
# used the earlier method for many years happily) because it seemed to be the case
# that with it enabled, ssh sessions would die if the connection dropped even
# briefly.
# It is unclear how long SSH waits if TCPKeepAlive is yes. The output of this
# command may give clues, at least on Linux computers:
#     grep -T . /proc/sys/net/ipv4/tcp_keepalive*
# In any case, normal users cannot change the duration if they use TCPKeepAlive yes
```

#### Host-specific Settings
Optional.

You can specify stanzas for specific hosts in order to create short aliases for those destinations, indicate the username you use there, and control which public key is used for access. (It is recommended to use different keys for different organizations.)

```Shell title="Per-host definitions for convenience"
Host jhpce01 j1 jhpce01.jhsph.edu
    Hostname jhpce01.jhsph.edu
    User your-cluster-username
    ForwardX11 yes
    IdentityFile ~/.ssh/id_ecdsa.jhpce
```

!!! Note "The above stanza allows you to use this command"
    `ssh j1`
    
    instead of this command:
    
    `ssh -X -i ~/.ssh/id_ecdsa.jhpce your-cluster-username@jhpce01.jhsph.edu`

#### Host-related Settings

Optional.

```Shell title="Host-related definitions for all hosts not explicitly mentioned earlier"
Host *
    ForwardX11 no
    IdentityFile ~/.ssh/id_ecdsa.my-general-key
```
