---
tags:
  - ai
  - topic-overview
---

# AI Overview

The AI section of our website and this document provide information about broad and specific AI topics for users of the JHPCE clusters: JHPCE (aka JASPER) and JADE. If you have suggested material to include, please send email to bitsupport@lists.jh.edu.

## Dangers accompanying opportunities

AI technology is a double-edged sword - along with positives there are negatives that you can - and should - avoid!!

Everyone is striving to integrate AI into, well, everything, and to make using it extremely simple. That has resulted in choices being made where doing "simple" things cause private data to become public or to generate a great number of activities "behind the scenes".

For example, ==providing research data in AI queries== can generate severe legal and moral consequences to Hopkins. There are subleties to how and where one uses AI to do their work. Hopkins is trying to create secure ways to work with certain classes of data, and provide guidance about the whole AI landscape.

Tools like `VS Code` can default to, or prompt you for permission to, do things which seem useful but have negative implications beyond your understanding at that moment. You might disable something like "security controls" because doing so means you don't have to respond to a prompt each time you enter a directory of files. But those controls also might require approval before the application deletes enormous numbers of your files. Perhaps all of the ones you have write access to, in the cluster, across multiple file systems. Do you collaborate with others using group-writable files and directories? Think about that for a moment!

## Hopkins AI tools & policies

The [JHU Generative AI Resources](https://jhpce.jhu.edu/help/images/JHU_Generative_AI_Resources.pdf) (PDF) document contains guidance about AI tools, costs and how to access them.

[Guidelines for Responsible Use of AI](https://it.johnshopkins.edu/ai/guidelines-for-responsible-use-of-ai/) applies to **you**, no matter which tool you use or where.

## Some specific things to avoid

??? warning "Do not run AI programs on login nodes. Only use compute nodes!!"
    Login nodes do not have a lot of RAM or CPU power. They are also critical to the entire user community. You must not intentionally or unintentionally spawn workloads on these servers.
    
??? danger "Do not create broad file search queries without careful consideration."
    Be very aware that you should start every interaction process with explicit limits in place. Tools like Codex make it _trivial_ to search for files which include "data" anywhere in their name or are of type "pdf". Our clusters host _millions of files_, occupying petabytes. If you do not add limits like the starting path for a search, these tools will start at the root (i.e. "/") of the file systems on the computer and go everywhere they have permission to go.

## Specific AI application usage in JHPCE: tutorials & guidance

In some cases, you can start applications using our web portals. These require you to be connected to a Hopkins institutional network, such as the VPN when working remotely.

!!! warning "Web portals: Note the job duration setting"
    Our web portals spawn SLURM jobs running as you. When launching a job, you have drop-down menus which allow you to request RAM, CPU and sometimes job durations. Two things about durations: (1) You should note when it will end and ensure you finish your work beforehand. (2) You might want to check using `squeue --me` that the job has ended, especially if you couldn't access the session or are disconnected because of a network or computer problem.

### VS Code from Microsoft

We have a [document](../ai/vscode.md) about this.
    
In the JHPCE/JASPER cluster, use our web portal to launch VS Code sessions. The web page for the portal is: [https://jhpce-app02.jhsph.edu/](https://jhpce-app02.jhsph.edu/)

In the JADE cluster, use our Open OnDemand web portal to launch VS Code sessions. The web page for the OOD portal is: [https://jade-ondemand01.jhsph.edu/](https://jade-ondemand01.jhsph.edu/)

### Codex from OpenAI

We have a [document](../ai/codex.md) about this.

### Claude

We have a [document](../ai/claude-example.md) about this.
