# Glossary

Terms you might wonder about. If you don't find what you're looking for, please use Google. Italicized terms indicate that there is a glossary entry for that item.

**Absolute path**
: A *path* which starts with a forward slash. Your shell (such as *bash*) interprets this location as starting with the root of the *file systems* on a *computer*. See also: *relative path*

**ACE**
: Access Control Entry. A component of an ACL. See [our ACL document](../files/acl.md).

**ACL**
: Access Control List. A collection of ACEs associated with a file or directory. Used to extend the normal UNIX permission scheme. See [our ACL document](../files/acl.md).

**bash**
: An example of a shell, which are programs which accept and process commands entered via text with a certain syntax, or set of rules. (Wikipedia [page](https://en.wikipedia.org/wiki/Bash_(Unix_shell)))

**CLI**
: Command Line Interface - a program like *bash* which provides a means to enter commands via text strings and see responses. (Wikipedia [page](https://en.wikipedia.org/wiki/Command-line_interface).)

**Cluster**
: A collection of computers (aka "*nodes*") managed as a group to provide services. Usually uses a *job scheduler*.

**Compute node**
: Computers within a *cluster* used to execute *jobs*

**Computer**
: You're using one right now. Aren't they marvelous?

**Core**
: A component of a *CPU* which processes *threads* of program instructions. Cores each have their own collections of components which are needed to execute instructions, such as registers for mathematical operations, memory stores to cache information. CPUs can contain many cores. Cores can have support for *hyperthreading*. (Wikipedia [page](https://en.wikipedia.org/wiki/Multi-core_processor). SLURM [support for cores/threads](https://slurm.schedmd.com/mc_support.html).)

**CPU**
: Acronym for Central Processing Unit. The main processor in a *computer*. Contains one or more *cores*. There may be more than one CPU within a computer. See information about the CPUs on your UNIX computer with the command `less /proc/cpuinfo` (press q to quit). ([Wikipedia page](https://en.wikipedia.org/wiki/Central_processing_unit))

**Current working directory**
: Expansion of acronym CWD. Your current (aka present) location within a *file system*. Can be expressed as a *path*. The command `pwd` will return the path to your CWD.

**EMACS**
: A super-complex but powerful and VERY extensible text editor written in Lisp, an abomination of a computer language. I called it "Eight Megabytes And Constantly Swapping" back in the day when a megabyte of RAM cost thousands and thousands of dollars. Like Doctor Who, proponents can be tiresome to others. See also: *vi*

**File system**
: A data storage service which allows for the storage and retrieval of information. File systems are basic units of storage. File system services include holding metadata about files, such as when they were created and who owns them. The command `df -h .` will return information about the file system of your *current working directory*. (Wikipedia [page](https://en.wikipedia.org/wiki/File_system))

**GUI**
: Acronym for Graphic User Interface. Antonym of *CLI*. A means of interacting with a user via windows. Operating systems such as macOS and Windows rely on GUI for its ease-of-use. X11 is a protocol which provides for the display of GUI windows over networks. Some of the *windows* in a GUI might be terminals offering CLI programs, such as *bash* shells. 

**Hyperthreading**
: Modern CPU *cores* normally have some additional circuitry which store information used by (normally two) threads. This allows the core to easily switch its attention between two threads so work can continue if one of the threads needs to wait for information to be brought into the core from RAM. (Wikipedia [page](https://en.wikipedia.org/wiki/Hyper-threading))

**Job**
: A unit of work given to an operating system by a *scheduler* on behalf of a user

**Job scheduler**
: Application which controls the execution of interactive or batch jobs on a cluster. SLURM is a *job scheduler*. (Wikipedia [page](https://en.wikipedia.org/wiki/Job_scheduler))

**Linux**
: A flavor of UNIX. Don't let them convince you that they invented it. (They will try. It's sad, really.)

**locality**
: The proximity of data stored in memory to the processing circuitry. You want to keep data as close as possible when creating *jobs*.

**Memory**
: General term for anything that can store information. You have a memory. Computers use many types of memory. People often mean *RAM* when they say memory. Sometimes they mean hard drive space when they say memory, which is understandable but confusing to those trying to help them. Say "disk space" or "disk storage" instead. Thanks.

**Memory hierachy**
: A hierarchy of types of *memory* used by a *computer*. Each type has different speed of access, capacity, and cost. Fast memory is expensive and usually provides for limited capacity. Each is therefore usually used for certain purposes. A register inside of a *core* inside of a CPU is the fastest memory, but small. Next is one or more caches inside of a core. Next (cheapest, fastest, most capacious) is *RAM*. After RAM comes hard drives on the same computer. After hard drives on the local computer comes hard drives on file servers which have to pass their information across the network. Next come USB sticks, then floppy disks, then humans typing into keyboards. The closer to a register you can keep the information needed for your program, the faster it can be processed. This distance from the processor is called *locality*.

**Pane**
: (Associate with *window*) A subdivision within a window where information is displayed, perhaps alone or perhaps alongside other panes

**Path**
: A string of characters used to identify a location in a directory structure inside of a file system. That location represents either a file or directory. Examples: `./my-script` or `/dcs07/a-groups-data/some/place/down/deep` See also: *absolute path*, *current working directory*, *relative path*.

**Node**
: Any computer within a *cluster* Nodes can be grouped by the services they provide, such as offering computation or file services. Usually used to indicate a *compute node.*

**RAM**
: Acronym for Read Access Memory. Part of a *memory hierarchy*. A form of computer memory that can be read or written in any order. Contrast with data stored on a tape. You cannot read any data from the tape without moving the tape to reach the portion where your desired information is stored. Some of us had to use data on tapes. We were lucky we didn't have to use data stored on punchcards.

: RAM is fast and capacious. Most RAM used in cluster nodes is volatile -- it cannot store information without continuous electrical power. (Wikipedia [page](https://en.wikipedia.org/wiki/Random-access_memory))

**Relative path**
: A path which does not start with a forward slash. Your shell (such as bash) interprets this location starting with your *current working directory*. See also: *absolute path*

**Scheduler**
: A component of a *job scheduler* application which decides which *jobs* to start running, when, and on which *cores* on which *CPUs* on which *nodes*. (Operating systems also contain schedulers, which do the same things for any process or thread, whether or not they are part of a job.) (SLURM CPU allocation process described [here](https://slurm.schedmd.com/cpu_management.html). SLURM has multiple other documents in the Slurm Scheduling section of this [page](https://slurm.schedmd.com/documentation.html).)

**SSH**
: Acronym for Secure SHell. Technically a protocol for multiple network services. Usually meant to refer to a program which implements that protocol to provide a *CLI*. See our SSH [document](../access/ssh.md). (Wikipedia [page](https://en.wikipedia.org/wiki/Secure_Shell))

**Thread**
: The smallest sequence of programmed instructions that can be managed independently. Part of a process (which may have one or more threads). It takes time for the information needed by a thread to run to be loaded into a CPU core. So there is an expense (delay) to switching between threads on the same core. *Hyperthreading* attempts to reduce that cost. (Wikipedia [page](https://en.wikipedia.org/wiki/Thread_(computing)). SLURM [support for cores/threads](https://slurm.schedmd.com/mc_support.html).)

**UNIX**
: A family of operating systems initially brought to you by Bell Telephone, a monopoly in the US. UNIX has fundamentally always used a *CLI* because, well, *GUIs* didn't exist for many years after UNIX got started. UNIX has a steep learning curve, but has only grown in use over the decades because it is powerful and extensible. If you're struggling to learn UNIX, remember that many people had to learn UNIX without the Internet or Google. Modern macOS and Windows (since Windows NT) rely on UNIX code. Linux is not UNIX. Linux is _a flavor_ of UNIX. Anyone who tells you otherwise is a poser. (Wikipedia [page](https://en.wikipedia.org/wiki/Unix))

**User**
: You

**Vi**
: The best text editor. Anyone who tells you otherwise is uninformed.

**Window**
: (Associate with pane) One or more panes displayed to a user by a software application.
