---
tags:
  - ai
  - topic-overview
---

# AI Overview

The AI section of our website and this document provide information about broad and specific AI topics for users of the JHPCE clusters: JHPCE (aka JASPER) and JADE. If you have suggested material to include, please send email to bitsupport@lists.jh.edu.

## Dangers accompany opportunities

AI technology is a double-edged sword - along with positives there are negatives that you can - and should - avoid!!

Everyone is striving to integrate AI into, well, everything, and to make using it extremely simple. That has resulted in choices being made _for you_ by AI vendors and plugin/extension authors where doing "simple" things can cause private data to become public or to generate a great number of activities "behind the scenes". All in the hopes of "being helpful", "reducing user workload", "enabling faster research."

For example, ==providing research data in AI queries== can generate severe legal and moral consequences to Hopkins. There are subleties to how and where one uses AI to do their work. The details in both data and software usage agreements matter a lot. Hopkins is trying to create secure ways to work with certain classes of data, and provide guidance about the whole AI landscape.

Tools like `VS Code` can default to, or prompt you for permission to, do things which seem useful but have negative implications which are unclear at that moment. You might enable or disable something quickly because you just want to get on with your work. A great example: We have seen Claude's "--dangerously-skip-permissions” flag being enabled (perhaps via a GUI checkbox whose label is more innocuous-sounding than the CLI string), because doing so means one doesn't have to respond to a prompt each time they enter a directory of files. But those controls also might normally require your approval before the application _deletes_ any of your files. Perhaps all of the ones you have write access to, in the cluster, across multiple file systems. Do you collaborate with others using group-writable files and directories? Think for a moment about all of those being deleted or modified!

## Hopkins AI tools & policies

The [JHU Generative AI Resources](https://jhpce.jhu.edu/help/images/JHU_Generative_AI_Resources.pdf) (PDF) document contains guidance about AI tools, costs and how to access them.

[Guidelines for Responsible Use of AI](https://it.johnshopkins.edu/ai/guidelines-for-responsible-use-of-ai/) applies to **you**, no matter which tool you use or where.

## Some specific things to avoid

??? warning "Do not run AI programs on login nodes. Only use compute nodes!! (Usually via web portals)"
    Login nodes have a limited amount of RAM and CPU resources. ==They are critical to the entire user community.== You must not intentionally or unintentionally spawn AI workloads on these servers.
    
??? danger "Do not create file search queries without careful consideration."
    Be very aware that you should start every AI interaction process with explicit limits in place. Tools like Codex make it _trivial_ to search for files which include some string such as "data" anywhere in their name or are of type "pdf". Our clusters host _millions of files_, occupying petabytes.
    If you do not add limits like the starting path for a file search, these tools will start at the root (i.e. "/") of the file systems on the computer and go everywhere they have permission to go.

In October 2026 a Claude user caused the file storage server holding everyone's home directories to crash. Their search looked for a single Python file, starting at "/" and going six levels deep in all accessible file systems. Which included all of the files on that particular compute node and all of those found on the NFS-mounted file systems. All of `/users/*`, all of `/dcs04/*`, all of `/dcs05/*`, all of `/dcs10`, all of `/dcs11/*` and all of `/jhpce/shared`...

## AI application usage in JHPCE clusters: tutorials & guidance

The web portal for the JHPCE (aka JASPER) cluster is [https://jhpce-app02.jhsph.edu/](https://jhpce-app02.jhsph.edu/)

The Open OnDemand (OOD) web portal for the JADE cluster is [https://jade-ondemand01.jhsph.edu/](https://jade-ondemand01.jhsph.edu/).

These portals are the approved way to run VS Code sessions on compute nodes. 

??? warning "You may need a JHED ID to run some AI tools."
    You can start some AI applications using our web portals. That is the supported way to run VS Code, for example. These servers require you to be connected to a Hopkins institutional network, such as the VPN when working remotely. Using the the Hopkins VPN requires your having a JHED (JH Enterprise Directory) account. You might need to work with a Hopkins department administrator to sponsor a JHED for you.

??? warning "Web portals: Note the job duration setting"
    Our web portals spawn SLURM jobs running as you. When launching a job, you have drop-down menus which allow you to request RAM, CPU and sometimes job durations.
    Two things about durations:
    (1) You should note when it will end, modify it if possible, and ensure you finish your work beforehand.
    (2) You might want to check using `squeue --me` that the job has ended, especially if you couldn't access the session or are disconnected because of a network or computer problem. Otherwise the job will continue running, which consumes community resources and accrues charges to your PI's budget. 

### VS Code from Microsoft

We have a [document](../ai/vscode.md) about this.

### Codex from OpenAI

We have a [document](../ai/codex.md) about this.

### Claude

We have a [document](../ai/claude-example.md) about this.
