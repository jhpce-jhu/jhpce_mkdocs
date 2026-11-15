---
tags:
  - ai
  - in-progress
  - claude
---

# Claude notes
Most users use Claude interactively. (It can also be run in [batch mode](running-claude-in-batch-jobs).) An example interactive Claude CLI session is included below which walks you through creating a simple Python script, submitting it as a Slurm job, monitoring the run, and gathering the output.

## Important warnings about using Claude on JHPCE

Claude is a very powerful tool in aiding with software development and data analysis, but it can often try to be too helpful.  This is fine when working locally on an individual laptop, but on a shared system like JHPCE, this helpfulness can overreach boundaries and affect other users.

### Do not run claude on login servers
Always run programs on compute nodes. The login servers are for small simple tasks.

### Control the scope of file searches

{==Please be sure to restrict Claude file access to only your home directory or project directories==} that your group has access to.  Please do not let Claude search for files on the entire system, or in whole file systems such as ""/users/"" or a project allocation space like "/dcs10", as this will put an undue load on the storage arrays, and impact the performance of the cluster for all users.

### dangerously-skip-permissions

A great example of people checking a box or adding an option in order to ease their use of Claude without considering more carefully the consequences.

We have seen Claude's {=="--dangerously-skip-permissions”==} flag being enabled (perhaps via a GUI checkbox whose label is more innocuous-sounding than the CLI string), because doing so means one doesn't have to respond to a prompt each time they enter a directory of files. But those controls also might normally require your approval before the application _deletes_ any of your files. Perhaps all of the ones you have write access to, in the cluster, across multiple file systems. Do you collaborate with others using group-writable files and directories? Think for a moment about all of those being deleted or modified! {==This has happened out in the real world.==}

## System Level Controls & Context Clues - IMPORTANT

!!! warning "System files impact your claude work"
    To try to prevent problems and improve application performance, we have deployed (in November 2026) two kinds of files in the directory `/etc/claude-code/` on our computers.

The two types are:

- "managed-settings" files, e.g. `managed-settings.d/10-common.json`
- "memory" files, e.g. `CLAUDE.md` & `node-rules-compute.md`

The managed-settings files are read *before the LLM begins*, which allows systems administrators to define rules. The informational memory files provide context to Claude.  The rules must be very concrete and specific, while the contextual prompts can contain both broad overviews as well as specific advice. Users can override informational file content, and some manage-settings rules.

If you believe that your ability to use Claude is impaired or prevented by the contents of these files, please send a description of the issue and the relevant contents of the system files to bitsupport@lists.jh.edu. We will review and possibly change the configuration. Suggestions are also welcome!!

## Interactive Claude Session Transcript

Here is an example Claude CLI session. It walks you through creating a simple Python script, submitting it as a Slurm job, monitoring the run, and gathering the output.

### Environment Notes

- Before starting Claude, an API license must be acquired through the Claude web interface.
- An API key is generated on the Claude web site and added to the file `~/.claude/settings.json` file
- To run Claude, you will need to `srun` into a compute node, {==load the `node` module==}, and run the `claude` command.
- Sessions run with workspace-write sandboxing and restricted network access.

### The Session

"user1" is the example user name -- you will see your own username. You can also do this work on our other login server, `jhpce03.jhsph.edu`.

```console
[user1@jhpce01 ~]$ srun --pty --x11 bash
[user1@compute-107 ~]$ module load claude
[user1@compute-107 ~]$ claude
╭─── Claude Code v2.1.150 ─────────────────────────────────────────────────────╮
│                                                    │ Tips for getting        │
│                Welcome back Bob!                │ started                 │
│                                                    │ Run /init to create a … │
│                       ▐▛███▜▌                      │ Note: You have launche… │
│                      ▝▜█████▛▘                     │ ─────────────────────── │
│                        ▘▘ ▝▝                       │ What's new              │
│                                                    │ Bug fixes and reliabil… │
│    Opus 4.7 (1M context) · API Usage Billing ·     │ Bug fixes and reliabil… │
│    Bob‘s Individual Org                            │ Added `fallbackModel` … │
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

## Running Claude In Batch Jobs

You can run Claude Code non-interactively inside a batch script using its headless print mode (-p).

Reasons to do this include analyzing large datasets, refactoring codebases in parallel via job arrays, or generating reports.

### Cautionary Notes

- Using Claude consumes tokens whether used interactively or in batch jobs. Interactive sessions give you feedback on how many tokens are being consumed, which allows you to cancel operations if things get out of hand. You won't get that feedback in batch jobs, so consider the potential impact of this.
- Internet access: Our compute nodes are able to reach the outside world. However, the JADE cluster is connected to the rest of campus by a tightly restricted network managed by IT@JH. The default security stance is deny-by-default for both TCP/IP ports and destination/source computers.

### Prerequisites

- Authentication: Export your ANTHROPIC_API_KEY in your environment or load it from a secure file (chmod 600) inside your script.
- Permissions. Pass `--dangerously-skip-permissions` (or configure `--allowedTools`) so Claude does not hang waiting for interactive terminal approval to read files or run bash commands.
- Remember to `module load claude`

### Example batch job

Pairing claude -p with #SBATCH --array=1-50 allows you to process dozens of independent files or repositories concurrently across compute nodes.

```
#!/bin/bash
#SBATCH --job-name=claude-batch
#SBATCH --output=logs/claude_%j.out
#SBATCH --error=logs/claude_%j.err
#SBATCH --time=01:00:00
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G

# 1. Load environment & API key
module load claude
export ANTHROPIC_API_KEY=$(cat ~/.anthropic_key)

# 2. Run Claude Code non-interactively
claude -p "Analyze the error logs in ./data/run_${SLURM_ARRAY_TASK_ID}.log and write a JSON summary to ./results/summary_${SLURM_ARRAY_TASK_ID}.json" \
  --allowedTools "Read,Write,Bash" \
  --output-format json
```

## Productivity Notes

- The claude command does not have a manual page but you can run `claude --help`.
- Claude stores its files in your home directory in a `./claude` subdirectory. You can find a copy of your session interactions in files with the `.jsonl` suffix.
- You can `srun` into a compute node, run claude, and, if you `/exit` out of claude, and then exit out of the compute node, you can later `srun` into a different compute node and run `claude --continue` or `claude --resume` and it will pick up from where you exited out. HOWEVER, resuming or continuing past sessions from longer than a day or so means all of the material will need to be re-read, which can burn through tokens at a high rate.

### Controlling Token Consumption
You have a limited number of tokens. Tokens cost money and enable productivity, so gauging token consumption is sometimes important. There are fairly simple techniques you can learn to dramatically reduce token consumption described elsewhere -- consider researching them! They can cut down consumption by factors of 2 to 20.

You can ask for token expenditures ...

#### In Interactive Sessions

- `/context` — a live breakdown of what's currently in the context window (system prompt, CLAUDE.md, skills, MCP tools, message history), with per-item token counts.
- `/usage` (alias `/cost`) — cumulative session totals: input, output, cache-read, cache-write tokens per model, plus an estimated dollar figure.

#### In Batch Job Sessions

It might be useful to save metadata about your Claude usage inside of batch jobs. Doing so would allow you to better understand why results didn't provide the desired results. It also allows you to see details like token consumption to help you budget tokens (and therefore dollars)

```
# Do your work
claude -p "your task here" --output-format json > result.json

# Then query context in the same session (note -c continues it)
echo "/context" | claude -p -c --output-format json --verbose | \
    jq -r '.[1].message.content'
```
The --output-format json on the main claude -p call also returns total_cost_usd and a per-model cost breakdown, so your batch script can log every run without needing a second call.

#### Advice For CLAUDE.md Contents

The single most important rule: keep it under 200 lines (Anthropic's own guidance).  It's injected into context on every turn, so a 5,000-token file costs 5,000 tokens before the user types a word.

Specific items that help in an HPC context:

- A "Compact instructions" section (what to preserve vs. discard on /compact) -- This prevents compaction from burying critical state.
- A "Token Budgets" section: per-task and per-session ceilings, with an instruction to self-report when approaching them -- Forces Claude to surface rather than silently consuming.
- Specific file paths to work with -- Vague prompts trigger broad grep/find scans that can burn tens of thousands of tokens.
- Exact commands with flags when known -- Saves a turn and many lines of output per session
- Answer budgets e.g. "Answer in five bullets max"
- Exclude long documentation, detailed manuals, frequently-changing task notes, full API docs, large code examples.
