# Using rclone to access OneDrive

Below is an example of using rclone to access the OneDrive network resource on the JHPCE cluster.  The initial setup is a bit involved, but [regular operation](#regular-operation) is fairly straightforward.

Before you start, you will need to have an X11 graphical environment set up either by using MobaXterm on a Windows system or Xquartz on a Mac.  To start, login to the cluster as normal, and then srun into a compute node with a 10G RAM request (srun –pty –x11 –mem=10G bash ) .  Part of the rclone setup process will involve using a web browser to generate an authentication key, so once you’ve logged into a compute node, run “chromium-browser &“.  The ampersand at the end will cause Firefox to run run in the background.  You may see a steam of warning message about “libGL errors”, but these are because we are using X11 forwarding and not a local graphics card, and assuming the browser stars up acter a few seconds, those messages can be ignored.

## Regular Operation
