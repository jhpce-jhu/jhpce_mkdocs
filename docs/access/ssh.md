# SSH - Key Information

## SSH Basics

Access to the JHPCE cluster requires the use of SSH.

SSH stands for Secure SHell. SSH is actually a set of internet standard protocols. Programs implementing these protocols include both command line interface (CLI) tools and those with graphic user interfaces (GUI).  They all enable you to make secure, encrypted connections from one computer to the next.

Depending on the kind of operating system your computer uses, you may or may not need to install SSH software.

Apple Macs come with CLI SSH tools pre-installed. You use them by entering commands in the Terminal app.  You can install GUI apps from various vendors, but we will only discuss the CLI tools except for some file transfer GUI programs.

On a Windows system you will need to install an ssh client.  We recommend the excellent GUI program MobaXterm. [Here is a document](mobaxterm.md) describing how to use it. There are other programs, such as the [PuTTY](https://en.wikipedia.org/wiki/PuTTY) family of tools and [WinSCP](https://en.wikipedia.org/wiki/WinSCP).

### SSH Keys
SSH programs make use of something called [public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography). Basically secure communications can be created by splitting a secret into to parts, and placing one part on each end of a link.

This can be extended to an optional pair of files you can generate and distribute such that one is located on the JHPCE cluster and the other is on your computer. Or smartphone!

This key pair of files is generated once, and protected with a passphrase. You place a copy of the "public" key file on JHPCE in a particular place with specific permissions. You keep a copy of the "private" key file on your personal device(s). Once you prove to a program (ssh-agent) running on your device that you know the passphrase to your private key file, it will thereafter provide your private key when you run an SSH command.

Once configured properly, you can use SSH keys instead of your JHPCE password.

[Example document](https://hpc-docs.cubi.bihealth.org/misc/ssh-basics/)

## Just yanked in here from our Knowledgebase document

#### Login howto
+ When ssh-ing into the cluster you will be prompted for 2 pieces of information. 
  + First you will be prompted for “Verification Code:”, and for this you will enter the 6 digit number from your authenticator app (we recommend Google authenticator).
  + Next you will be prompted for “Password:”; this you this will use a traditional password that only you know.  *Note that when entering your Verification Code and Password, your cursor will not move when you type, so it will appear like nothing is happening*.  + If you have never accessed the cluster before, and “Initial Verification Code” and “Initial Password” will be provided to you during your JHPCE Cluster Orientation session.
+ If you only get prompted for “Password” and not “Verification Code”, you are likely using the wrong login ID.  Your login ID is provided to you during the JHPCE orientation; it is not the same as your JHED ID.
+ If you have entered your credentials correctly, you will see a login banner for the JHPCE cluster, and you will be sitting at a shell prompt on the login node.  However, if you are prompted for “Verification Code:” again, you likely mistyped either your password or Verification Code, and you should wait until your Google Authenticator code changes (it changes every 30 seconds) and you will need to try logging in again.
+ If you have tried several times to login, and you try to ssh again and get a “Connection Refused” message, you will need to wait 30 minutes before trying to login again.
+ If you continue to have login issues, please contact bitsupport@lists.jhu.edu.

- [2 Factor Authentication](https://jhpce.jhu.edu/knowledge-base/2-factor-authentication/)


### Unix-based machines (linux and mac osx)

(((((((IN 2024 DO WE NEED TO SPECIFY A DIFFERENT (STRONGER) ENCRYPTION TYPE???)))))))
+ First, on your local laptop/desktop, open a terminal and `cd` into your home directory  and invoke the ssh key generator:

```
ssh-keygen -t rsa
```

+ You will be prompted for a passphrase.  For security reasons, we require that you use a passphrase to protect your key.  This prevents someone who gets access to your private key file from being able to use it!!!!! For the other questions, you can select the default values.
+ The key generator will put two files in the .ssh subdirectory of your home directory, typically  `~/.ssh/id_rsa` and  `~/.ssh/id_rsa.pub`. 
+ *You should only ever have to run the ssh key generator once on your local host*.  If you have already configured passwordless login and you run the key generator a second time, it will overwrite your previous public and private key files. This will break all password-less logins that you set up with your previous keys.
+ Next you want to copy your public key file to the remote host and append it to the authorized_keys file in the `.ssh` subdirectory of your home directory on the remote host. 
  + If there is no `~/.ssh` directory in the remote host, you will need to login to the remote host and create one. Note, attempting to connect to another host, like `ssh github.com`, will create one if it isn't there already. 
+ You can perform the copy and append operations in one line as follows;

```
cat ~/.ssh/id_rsa.pub | ssh <your_userid>@jhpce01.jhsph.edu 'cat >> ~/.ssh/authorized_keys'
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

(((This isn't true if you use `srun`. Is it even true in JHPCE 3.0 anyway?  Also, do we want to be suggesting that people log into compute nodes?)))

Logging into a cluster node from a login node requires keypairs. 
 If
your private key file is in `.ssh/` then it should work, since the
public key file is in your `authorized_keys` file. If you do not want
the same private key file to be used to log into nodes as the one used
to log into the login node, then repeat create a new public private
key / public key pair on the cluster.  Append the public key to
your`authorized_key` file. Note, when appending, add the public key,
do not overwrite the existing file.
  
#### Troubleshooting

1. Many users set up an alias for the ssh command so they don’t have to type as much to log into the remote host.  You can do this by adding the following line to your `~/.bashrc`, `alias hpc='ssh -X <your_userid>@jhpce01.jhsph.edu'`.
2. If you followed the directions and ssh is still asking for a password, then it is likely that the permissions of the  `~/.ssh` directory  on the remote host, are not set correctly. To fix the permissions, execute the following command on the remote host `chmod -R o-rwx ~/.ssh`
3. Finally, if your changed the permissions of your `.ssh` directory
   and you are still being asked for a password, then perhaps the
   permissions on your home directory are such that others are allowed
   to write to it. If this is the case, then this needs to be changed
   so that only you can write to it. Contact bit support to have this
   changed.

### Windows machines 
If your desktop/laptop runs Microsoft Windows then you first need to
install MobaXterm on your windows machine. If you are
using MobaXterm, please use the steps at the bottom of the MobaXterm
Configuration Page.

## SSH and X11

When -X ?
When -Y ?

XQuartz is an optional Mac program you have to instal 

MobaXterm contains an X11 client

How do Windows users configure ForwardX11Timeout ?

Is `ForwardX11Timeout 0` better in 2024 than specifying a particular time in hours?

https://jhpce.jhu.edu/question/my-x11-forwarding-stops-working-after-20-minutes/

## Mac-specific Configuration

In your ~/.ssh/config file

UseKeychain yes
XAuthLocation /opt/X11/bin/xauth
#
AddKeysToAgent yes
IdentityFile ~/.ssh/id_ecdsa
IdentityFile ~/.ssh/id_rsa
ForwardAgent yes
