Our younger users haven't necessarily ever built anything from source. Or on machines owned by someone else where you cannot use sudo dnf install blah. We can encourage them that it is possible, give them a few pointers to doing it successfully.

For example, telling them that they will have to modify the destination location from the typical /usr/local/bin and that you can usually do that easily in the config stage?

Do it on a compute node in an interactive shell.

If we don't write something, then we can provide pointers to the work of others. If so, that would probably be inside a larger document.

[Software installation with Conda](https://hpc-docs.cubi.bihealth.org/best-practice/software-installation-with-conda/)

[Example page](https://hpc-docs.cubi.bihealth.org/how-to/software/scientific-software/)

# **Please visit the links below, poke around to nearby topics on those site pages. If you were a new JHPCE user and new to UNIX and HPC, what would you like to see, using these examples?
**

Maybe the info JRT has collected below about toolchains, maybe these particular URL compared to others on those sites, should be inserted into a github issue and considered later. JRT wonders if we should begin to adopt the practice of being more specific about creating and using toolchains.

Note that these clusters provide modules with toolchain information. AFAIK we've never named our modules with toolchain information (with which compiler they were built). Does ARCH? A lot of this seems to be Intel vs gcc. But wouldn't it help us keep our modules updated if we embedded the compiler generation they were built with into their name?

These places give date names to collections of tools. Yale's [toolchain](https://docs.ycrc.yale.edu/clusters-at-yale/applications/toolchains/) page has an illustration of "foss" versus "foss-cuda"

Example from [C.E.C.I](https://support.ceci-hpc.be/doc/_contents/UsingSoftwareAndLibraries/CompilingSoftwareFromSources/index.html)

Another example for [frontera cluster](https://docs.tacc.utexas.edu/hpc/frontera/#building) Very nice info to help you optimize your code. Advice about locality, I/O performance, machine learning etc nearby in sidebar.

Another example from [Yale](https://docs.ycrc.yale.edu/clusters-at-yale/applications/compile/) warns users about considering node CPU capabilities. (So does Frontera where you get practical info about gcc -march and -mtune)
