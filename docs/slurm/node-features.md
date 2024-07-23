---
tags:
  - slurm
---

# Node Features

!!! Warning
    For advanced users who want the highest performance at the expense of possibly waiting longer for jobs to start and maintaining multiple versions of code. We cannot advise you on building optimized code.

JHPCE has a wide variety of compute nodes. Some have GPU processors, some have large amounts of memory, and there are many models of CPUs present. We tend to keep equipment running as long as possible.

SLURM allows for the assignment of keywords to nodes so that users can guide their jobs to appropriate machines. This is, of course, in addition to the other methods of indicating resource needs, such as desired partition(s), amount of memory, and the need for a GPU.

If you believe that your code will gain a lot from being run on specific kinds of CPUs, you can compile it with optimization flags to perform well on them. Of course, you have then created binaries which might fail to run altogether on older nodes. Please name your binaries in a way which makes their special status apparent to you down the road, in case you forget. That might be a challenging puzzle to figure out.

One could, at run-time, write batch job scripts which check the features of the node they were assigned, and choose to execute code highly-optimized for that node's CPU.

!!! Tip
    See most available tag info for the cluster's node with
    `sinfo -o "%25N %50f %20G" | less`
## Current tag categories
Currently we have defined features for:

- CPU manufacturer (intel,amd)
- CPU line (e.g. Opteron)[^1]
- CPU *cpu-type* architecture as used by the gcc compiler (see [below](../slurm/node-features.md/#cpu-types))
- presence of a GPU
- GPU model
- GPU with memory amount
- RAM amount in 250Gb increments at 500 and above[^1]

[^1]: It's not clear that these tags add much value. The RAM tags were thought to perhaps aid someone trying to increase memory locality. SLURM's scheduler, will, of course, allocate nodes with enough free memory to meet the jobs requirements.

## Using Features with SLURM Jobs

You can tell the scheduler that you **prefer** or **require** features using the `--prefer=string` or `--constraint=string` when using `srun` or `sbatch` or `salloc`

Of course, saying that you **prefer** a feature means that your job might be sent to nodes which lack that feature. If your code **requires** a feature, then this will mean that your job has fewer nodes on which it might run and might stay pending for longer.

### Logical operators
See the `--prefer` and `--constraint` sections of the [sbatch](https://slurm.schedmd.com/archive/slurm-22.05.9/sbatch.html) manual page for descriptions of using AND and OR operators to combine features.

### Excluding Nodes
Sadly it does not seem possible to simply exclude features (so you could say I want any node except ones with these features (because they are too old for my code)). However, you can tell `srun` and `sbatch` to exclude one or more hosts with the `--exclude=` directive. You can provide a comma-seperated list of hostnames or a text file name (although this file name needs to include a ++slash++ in it).

!!! Tip
    When specifying multiple features, you may need to enclose them in double quotes. The symbols used for AND, OR and number of matches are all ones which the bash shell will mis-interpret unless told to not process them.

## Viewing Features

These features keywords are up to systems administrators to choose and define by adding "Features=string1,string2" to "NodeName=" entries in `/etc/slurm/nodes.conf`

For any particular node, you can see features information with the command `scontrol show node nodename`

If you specified a feature for a job, it will appear in the output of `scontrol show job jobid`

## CPU-Related Feature Info

### Manufacturer

JHPCE has CPUs made by both Intel and AMD.

So one feature that has been defined for our nodes is the CPU manufacturer: `amd` or `intel`

### Instruction Set Extensions

As time has passed, computer science has progressed and users have needed new kinds of computation to be as fast as possible, CPU manufacturers have decided that it is worth extending the assembly language used in central processing units because there were significant gains for notable use cases. Nothing is faster than silicon inside of a CPU core, so moving frequent calculations into CPU designs provides notable performance gains for some code sets.

The x86 instruction set is used on all JHPCE nodes. It has a number of extensions. The [x86 Wikipedia page](https://en.wikipedia.org/wiki/X86) includes pointers to them.

Extensions are listed as "flags" when you look at the output of `less /proc/cpuinfo` on a particular node.

### CPU "Types"

Instead of listing all of the extensions as features, we instead use *cpu-type* as defined by the authors of gcc.

The GNU C/C++ compiler suite is the most commonly used one for those languages. See [this page](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html) for cpu-type details, including which extensions each type represents.

This command was run on each node to determine its cpu-type:

`/usr/bin/gcc -march=native -Q --help=target | grep -- '^[ ]*-march' | cut -f3`

## Building Optimized Code

That is beyond the scope of this document.

“mtune” and “march” are important gcc flags to investigate.

The various levels of optimization requested via `-O#` might also impact the flexibility of the binaries you build.

!!! Note
    If you find good documents that would help other users optimize their binaries, please let us know at the bitsupport address.

If you want to create batch scripts which can switch between binaries at run-time, there are several techniques you can try:
1)	If you use `-—constraint` and your job starts, then (we believe) it will be running on the requested feature. (We haven’t tested to ensure that that is 100% respected. It’s _supposed to be_ (as opposed to using `-—prefer`). Therefore you can also pass a variable into your job with the `-—export` flag as explained in [this page](../slurm/environment.md)
2)	If you  do some output munging with regular expression matching, you can get the info out of your environment at runtime. You DO see the specified feature in the output of `scontrol show job $SLURM_JOBID` as `Features=amd` But you have to search for it in the output AND it only shows you the feature you specified, not all of those applicable to that node.
3) You could also examine the available features of the node on which your shell is running in the `ActiveFeatures` attribute in the output of `scontrol show node $SLURMD_NODENAME`


## Frequency By CPU-type

In August 2024, the cluster compute nodes consisted of approximately

```
      3 bdver1
      7 bdver2
     10 broadwell
      2 cascadelake
     11 haswell
     17 icelake-server
     16 sapphirerapids
      5 skylake-avx512
      4 znver2
      2 znver3
```

The bdver and znver types represent AMD CPUs.

Perhaps the largest delta in compiler flags is between bdver1 and the rest. bdver1 does not support these gcc flags:
  **-mbmi
  -mf16c
  -mfma
  -mdirect-extern-access**
