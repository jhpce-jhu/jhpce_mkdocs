---
tags:
  - in-progress
  - slurm
  - jeffrey
---

# Interactive Jobs

!!! Note "Authoring Note"
    It is not yet clear whether there is enough material to warrant a dedicated page for this topic.
    

The core command to create an interactive session is:

`srun --pty --x11 bash`

The last element of an srun command has to be a program.

The `--x11` is optional, and only needed if you are going to be [running X11 programs](../access/x11.md) during your session. Omitting it can avoid a number of issues.


## Shortcuts

You can define aliases bash routines in your .bashrc file for commands you use frequently.  Bash functions can be useful for commands that are difficult to use in simple shell aliases. You can view the definition of aliases with `alias` and bash routines with the command `declare -f function_name`.

**jsrun** - JHPCE srun with X11 support:
```
jsrun() { if [ -z ${DISPLAY} ]; then /usr/bin/srun --pty "$@" bash; else /usr/bin/srun --pty --x11 "$@" bash; fi }
```

**jxrun** - JHPCE srun when you want to skip X11 entirely:

```jxrun() { /usr/bin/srun --pty "$@" bash; }```

These examples inclue `$@` symbols, which are replaced by any additional arguments you provide. The srun command requires bash or another program to come last, which is one of the reasons why a simple shell alias can't be used. You have to create either a shell script or a routine.

So you can use these like a normal srun command, e.g.

`jsrun --mem=20G -p interactive`
