---
tags:
  - ai
  - in-progress
  - claude
---

# Claude notes

## Important warnings about using Claude on JHPCE

Please be sure to restrict Claude file access to only your home directory or project directories that your group has access to.  Please do not let Claude search for files on the entire system, or in whole file systems such as /users/ or a project allocation space like "/dcs10", as this will put an undue load on the storage arrays, and impact the performance of the cluster for all users.

Claude is a very powerful tool in aiding with software development and data analysis, but it can often try to be too helpful.  This is fine when working locally on an individual laptop, but on a shared system like JHPCE, this helpfulness can overreach boundaries and affect other users.

## Claude Transcript

An example Claude CLI session is shown below which walks you through creating a simple Python script, submitting it as a Slurm job, monitoring the run, and gathering the output.

### Environment Notes

- Before starting Claude, an API license must be acquired through the Claude web interface.
- An API key is generated on the Claude web site and added to the file `~/.claude/settings.json` file
- To run Claude, you will need to `srun` into a compute node, load the `node` module, and run the `claude` command.
- Sessions run with workspace-write sandboxing and restricted network access.

### The Session

"user1" is the user name.

```console
[user1@jhpce01 ~]$ srun --pty --x11 bash
[user1@compute-107 ~]$ module load node
[user1@compute-107 ~]$ claude
╭─── Claude Code v2.1.150 ─────────────────────────────────────────────────────╮
│                                                    │ Tips for getting        │
│                Welcome back Marcus!                │ started                 │
│                                                    │ Run /init to create a … │
│                       ▐▛███▜▌                      │ Note: You have launche… │
│                      ▝▜█████▛▘                     │ ─────────────────────── │
│                        ▘▘ ▝▝                       │ What's new              │
│                                                    │ Bug fixes and reliabil… │
│    Opus 4.7 (1M context) · API Usage Billing ·     │ Bug fixes and reliabil… │
│    Marcus‘s Individual Org                         │ Added `fallbackModel` … │
│                  /users/user1                      │ /release-notes for more │
╰──────────────────────────────────────────────────────────────────────────────╯
>  
```

Next we create a simple program and submit it as a job on the cluster.

```console
❯ please make a directory called claude-demo                                    

● Bash(mkdir /users/user1/claude-demo)
  ⎿  Done

● Created /users/user1/claude-demo.

✻ Brewed for 5s

❯ cd claude-demo                                                                

● Bash(cd /users/user1/claude-demo && pwd)
  ⎿  /users/user1/claude-demo
     
● Now in /users/user1/claude-demo.

✻ Brewed for 4s

❯ create a python script called count.py that prints the numbers from 1 to 10.  

● Write(count.py)
  ⎿  Wrote 2 lines to count.py
      1 for i in range(1, 11):
      2     print(i)

● Created count.py.

✻ Baked for 6s

❯ submit count.py as a slurm job                                                

● Bash(which sbatch && sinfo 2>/dev/null | head -20)
  ⎿  /usr/bin/sbatch
     PARTITION          AVAIL  TIMELIMIT  NODES  STATE NODELIST
     cluster-is-jhpce    down 90-00:00:0      1   idle compute-100
     … +18 lines (ctrl+o to expand)
     
● Write(count.sbatch)
  ⎿  Wrote 10 lines to count.sbatch
       1 #!/bin/bash
       2 #SBATCH --job-name=count
       3 #SBATCH --partition=shared
       4 #SBATCH --time=00:05:00
       5 #SBATCH --mem=1G
       6 #SBATCH --cpus-per-task=1
       7 #SBATCH --output=count_%j.out
       8 #SBATCH --error=count_%j.err
       9 
      10 python count.py                                     

● Bash(sbatch count.sbatch)
  ⎿  Submitted batch job 32459003
     
● Submitted job 32459003 to the shared partition. Output will land in
  count_32459003.out once it runs.
     
✻ Crunched for 13s
  
❯ show me the result of the job                                                 

  Read 1 file (ctrl+o to expand)

● Job output:

  1
  2  
  3
  4
  5
  6
  7
  8
  9
  10

✻ Brewed for 5s

※ recap: You created a claude-demo directory with count.py (prints 1-10) and 
  submitted it as a SLURM job, which completed successfully. Next: decide what 
  to do with the demo, or move on to another task. (disable recaps in /config)
  
```

### Productivity Note

Claude stores its files in your home directory in a `./claude` subdirectory. You can find a copy of your session interactions in files with the `.jsonl` suffix.

You can `srun` into a compute node, run claude, and, if you `/exit` out of claude, and then exit out of the compute node, you can later `srun` into a different compute node and run `claude --continue` or `claude --resume` and it will pick up from where you exited out.
