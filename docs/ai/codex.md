---
tags:
  - ai
  - in-progress
  - codex
---

# Important note about using Codex on JHPCE

Please be sure to restrict Codex file access to only your home directory
or project directories that your group has access to.  Please do not let Codex
search for files on the entire system, or the /users home directory space, as
this can put an undue load on the storage arrays, and impact the performance
of the cluster for all users.

Codex is a very powerful tool in aiding with software development and data
analysis, but it can often try to be too helpful.  This is fine when working
locally on an individual laptop, but on a shared system like JHPCE, this 
helpfulness can overreach boundaries and affect other users.

# Codex Session Transcript

This page captures a Codex CLI session that walks through creating a simple Python script, submitting it as a Slurm job, monitoring the run, and gathering the output. It is formatted for inclusion in an MkDocs-based documentation site.

## Environment Notes

- Session ran inside `/users/user1` with workspace-write sandboxing and restricted network access.
- No permission prompts or sandbox escalations were needed; every command executed inside the default sandbox.
- Prior to running this, an OpenAI license will be needed. One way to do this is to request an API key. The JHU Engineering department has a site to request an OPENAI API key [here](https://engineering.jhu.edu/cmts/jhu-openai-platform-api-request/) 
- Once you have a key, you can enter it when prompted after you first run "codex", or add it into the file ~/.codex/auth.json
- To running codex, you will need to srun into a compute node, load the "node" module, and run the "codex" command.
.
```console
[user1@jhpce01 ~]$ srun --pty --x11 bash
[user1@compute-120 ~]$ module load node
[user1@compute-120 ~]$ codex
╭─────────────────────────────────────────────────╮
│ >_ OpenAI Codex (v0.63.0)                       │
│                                                 │
│ model:     gpt-5.1-codex low   /model to change │
│ directory: ~                                    │
╰─────────────────────────────────────────────────╯

  To get started, describe a task or try one of these commands:

  /init - create an AGENTS.md file with instructions for Codex
  /status - show current session configuration
  /approvals - choose what Codex can do without approval
  /model - choose what model and reasoning effort to use
  /review - review any changes and find issues

 
› 
```

Below is an example of using codex to create a program and submit it as a job on the JHPCE cluster.

## 1. Create the Python Program

**User Prompt:** “please write a python program called count.py that will print the numbers from 1 to 10.”  
**Codex Response:** Generated `count.py` containing:

```python
for number in range(1, 11):
    print(number)
```

*Explanation:* Codex wrote a simple loop that prints integers 1 through 10 and stored it at `/users/user1/count.py`.

## 2. Submit the Program as a Slurm Job

**User Prompt:** “please run count.py as a slurm job on the cluster.”  
**Codex Response:** After confirming permission to run `sbatch`, Codex submitted:

```bash
sbatch --wrap "python /users/user1/count.py"
```

*Output:* `Submitted batch job 29465151`  
*Explanation:* The `--wrap` flag lets us wrap a short command line without writing a separate Slurm script.

## 3. Monitor the Job

**User Prompt:** “please monitor this jobs.”  
**Codex Commands:**

```bash
squeue -j 29465151
```

- First check: job pending (`PD`) in the shared partition.  
- Second check: job completed (`CG`) on node `compute-119`.

*Explanation:* `squeue` was run twice to show the transition from pending to completed.

## 4. Retrieve the Output

**User Prompt:** “please show the results from running this job.”  
**Codex Command:**

```bash
cat slurm-29465151.out
```

*Output:*
```
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
```

*Explanation:* Slurm wrote the Python program’s stdout to the default `slurm-29465151.out` file, confirming the script executed correctly.

