# SOFTWARE TOPIC - page to develop outline

## What we've installed & how to use it
### Overview
### Modules
### In the operating system
### Suggested programs for GUI use
See chart Jeffrey includes in C-SUB orientation

### Software by category
Example from [C.E.C.I](https://support.ceci-hpc.be/doc/_contents/UsingSoftwareAndLibraries/SoftwareInstalled/categories_items.html)

## How to add your own packages to R and python
Example from [C.E.C.I](https://support.ceci-hpc.be/doc/_contents/UsingSoftwareAndLibraries/InstallingSoftwareByYourself/index.html)

## Building your own software
Note that these clusters provide modules with toolchain information. AFAIK we've never named our modules with toolchain information (with which compiler they were built). Does ARCH? A lot of this seems to be Intel vs gcc. But wouldn't it help us keep our modules updated if we embedded the compiler generation they were built with into their name? These places give date names to collections of tools. Yale's [toolchain](https://docs.ycrc.yale.edu/clusters-at-yale/applications/toolchains/) page has an illustration of "foss" versus "foss-cuda"

Example from [C.E.C.I](https://support.ceci-hpc.be/doc/_contents/UsingSoftwareAndLibraries/CompilingSoftwareFromSources/index.html)

Another example for [frontera cluster](https://docs.tacc.utexas.edu/hpc/frontera/#building) Very nice info to help you optimize your code. Advice about locality, I/O performance, machine learning etc nearby in sidebar.

Another example from [Yale](https://docs.ycrc.yale.edu/clusters-at-yale/applications/compile/) warns users about considering node CPU capabilities. (So does Frontera where you get practical info about gcc -march and -mtune)