---
tags:
  - topic-overview
  - needs-to-be-written
  - gpu
  - mark
---

# GPU Critical Knowledge

## Overview or Everything in One Document??

Should this be an overview? JRT thinks so, because he envisions a growing number of GPU-related documents instead of one massively long document. 

Therefore should it be moved out of "User Guides" to be a topic inside of "Software"?

## MIGRATE PREVIOUS WEB SITE DOCUMENT

Probably put some of it here, some of it into other document(s)
https://jhpce.jhu.edu/knowledge-base/gpus-on-the-jhpce-cluster/

## Our GPU Nodes
Table with their resources, and partition names. Differentiate between public and PI partitions. Point to this [existing](../slurm/partitions.md#gpu-partitions) page.

Mention `slurmpic -g`

## Using them

Dos and don'ts.

Is it all interactive? If not, provide some example batch job files.

Mark has written material and sent it to various people. Combine that with perhaps info from this
[Tensorflow sample document](https://hpc-docs.cubi.bihealth.org/how-to/software/tensorflow/)

Look for other resources out there containing advice about using GPUs in a SLURM context.

## Existing docs at other sites 

!!! Warning
    These other clusters have different software and policies. Look for useful information but don't expect what they say/do to work here.
    
USC: [USC documentation](https://www.carc.usc.edu/user-information/user-guides/software-and-programming/using-gpus)


New Mexico State Univ: [their page](https://hpc.nmsu.edu/discovery/slurm/gpu-jobs/)

UMich section showing [relevant SLURM directives for GPU use](https://arc.umich.edu/greatlakes/slurm-user-guide/)

Yale has CUDA, tensorflow and miniconda modules while we do not. U[seful?](https://docs.ycrc.yale.edu/clusters-at-yale/guides/gpus-cuda/) [PyTorch](https://docs.ycrc.yale.edu/clusters-at-yale/guides/pytorch/) install instructions.

## Application-specific advice

!!! Note "Authoring Note"
    JRT added this info here but believes it should be migrated out of the "GPU Overview" document to somewhere else.

### Alphafold

```Shell 
$ module load alphafold/4.3.1
$ srun --pty --x11 --mem=100G --cpus-per-task=8 --partition gpu --gpus=1 bash
[compute-123]$ module load alphafold
(alphafold-gpu) [compute-123]$ run_alphafold.sh -d /legacy/alphafold/data -f ./test.fasta -o . -t 2020-05-14 -n 8
```

### Tensorflow
