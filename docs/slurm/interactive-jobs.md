---
tags:
  - slurm
  - jeffrey
---

# Interactive Jobs  

## The essentials
The core command to create an interactive session is:

`srun --pty --x11 bash`

The last element of an srun command has to be a program.

The `--x11` is optional, and only needed if you are going to be [running X11 programs](../access/x11.md) during your session. Omitting it can avoid a number of issues.


## Shortcuts

!!! Warning
    If you want to type 5 characters instead of 22 every time you want to create an interactive job, read on and indicate that this page was helpful.  If you like to type every last character of every command, skip this section.

Here are two equivalent commands. Which would you rather use?
: `srun --pty --x11 --mem=20G -p interactive bash`
: `jsrun --mem=20G -p interactive`

You can define aliases and bash routines in your `.bashrc` file for commands you use frequently.  Bash functions can be useful for commands that are difficult to specify in simple shell aliases. You can view the definition of aliases with `alias` and bash routines with the command `declare -f function_name`.   Changes to your `.bashrc` file aren't available to your currently running shell unless you log out and back in again, launch a new shell on top of the old one, or simply have your current shell re-read the file ("source it") with `. .bashrc` The following routines can be copied to your buffer by hovering your mouse over the right-hand side and clicking on the copy icon which appears (faintly).

**jsrun** - JHPCE srun with X11 support:
```
jsrun() { if [ -z ${DISPLAY} ]; then /usr/bin/srun --pty "$@" bash; else /usr/bin/srun --pty --x11 "$@" bash; fi }
```

**jxrun** - JHPCE srun when you want to skip X11 entirely:

```jxrun() { /usr/bin/srun --pty "$@" bash; }```

These examples inclue `$@` symbols, which are replaced by any additional arguments you provide. The srun command requires bash or another program to come last, which is one of the reasons why a simple shell alias can't be used. You have to create either a shell script or a routine.

So you can use these functions like a normal srun command, e.g.

```jsrun --mem=3G -p transfer```
