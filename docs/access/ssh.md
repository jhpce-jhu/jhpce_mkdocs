# SSH - all about it

[Example document](https://hpc-docs.cubi.bihealth.org/misc/ssh-basics/)

### Multi-factor authentication
### SSH Keys
### Mac and Linux Users
### Windows Users
Mention Putty and friends.
MobaXterm is recommended. Point to MobaXterm document.

### Screen and Tmux
[Example document](https://hpc-docs.cubi.bihealth.org/best-practice/screen-tmux/) mentioning these tools.

We should add whatever admonitions we want users to know about.

## Just yanked in here from our Knowledgebase document

## Authentication
+ Access to the JHPCE cluster is provided via SSH.  
+ SSH allows you to establish a secure, encrypted connection between your laptop and a remote computer.
+  How you initiate the ssh connection is going to depend on what type of local laptop or desktop you are using.  
   + If you are on a Mac computes, you can start a Terminal which already has ssh.  
   + On a Windows system you will need to install an ssh client.  We recommend MobaXterm (https://jhpce.jhu.edu/knowledge-base/mobaxterm-configuration/)
+ We require that all users use enable 2 Factor Authentication on their JHPCE account. 

#### Login howto
+ When ssh-ing into the cluster you will be prompted for 2 pieces of information. 
  + First you will be prompted for “Verification Code:”, and for this you will enter the 6 digit number from your authenticator app (we recommend Google authenticator).
  + Next you will be prompted for “Password:”; this you this will use a traditional password that only you know.  *Note that when entering your Verification Code and Password, your cursor will not move when you type, so it will appear like nothing is happening*.  + If you have never accessed the cluster before, and “Initial Verification Code” and “Initial Password” will be provided to you during your JHPCE Cluster Orientation session.
+ If you only get prompted for “Password” and not “Verification Code”, you are likely using the wrong login ID.  Your login ID is provided to you during the JHPCE orientation; it is not the same as your JHED ID.
+ If you have entered your credentials correctly, you will see a login banner for the JHPCE cluster, and you will be sitting at a shell prompt on the login node.  However, if you are prompted for “Verification Code:” again, you likely mistyped either your password or Verification Code, and you should wait until your Google Authenticator code changes (it changes every 30 seconds) and you will need to try logging in again.
+ If you have tried several times to login, and you try to ssh again and get a “Connection Refused” message, you will need to wait 30 minutes before trying to login again.
+ If you continue to have login issues, please contact bitsupport@lists.jhu.edu.

- [2 Factor Authentication](https://jhpce.jhu.edu/knowledge-base/2-factor-authentication/)

## SSH
We also support and recommend the use of SSH public/private key authentication to access your account. 

### Unix-based machines (linux and mac osx)
+ First, on your local laptop/desktop, open a terminal and `cd` into your home directory  and invoke the ssh key generator:

```
ssh-keygen -t rsa
```

+ You will be prompted for a passphrase.  For security reasons, we recommend that you use a passphrase to protect your key.  For the other questions, you can select the default values.
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
Logging into a cluster node from a login node requires keypairs. If
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

