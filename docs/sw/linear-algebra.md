---
tags:
  - obsolete
---

# Numerical Linear Algebra Libraries
If you are compiling your own numerical packages for R, Python, Perl,
etc. then you will more than likely need to use the BLAS, LAPACK or
ATLAS libraries. The purpose and relationships between these librareis
are as follows

### BLAS
BLAS provides basic building blocks for performing basic scalar,
vector, and matric operations, e.g. multiplication, addition,
subtraction, etc. In particular

+ Level 1 BLAS perform scalar, vector and vector-vector operations,
+ Level 2 BLAS perform matrix-vector operations
+ Level 3 BLAS perform matrix-matrix operations

BLAS is very mature, efficient and portable and used in
essentially all high quality numerical code that performs linear algebra

Default BLAS shared object libraries are at:

```
/usr/lib64/libblas.so.3.2
/usr/lib64/libblas.so.3.2.1
/usr/lib64/libblas.so.3
```

### LAPACK

LAPACK largely replaced fortran libraries originally developed in the
1970s (LINPACK and EISPACK), and takes advantage of level 2 and level
3 BLAS for efficient operation on modern multi-core architectures that
incorporate hierarcharical and shared memory.  See
[http://www.netlib.org/lapack/](http://www.netlib.org/lapack/) for documentation.

Default LAPACK shared object libraries are at:

```
/usr/lib64/liblapack.so.3.2.1
/usr/lib64/liblapack.so.3.2
ATLAS
```

### ATLAS
The ATLAS libraries consist of the BLAS and a subset of the LAPACK
routines.  ATLAS libraries are tuned to particular architectures and
compilers in an HPC environment.

Default ATLAS shared object libraries are at:

```
/usr/lib64/atlas
```

See [http://math-atlas.sourceforge.net/](http://math-atlas.sourceforge.net/) for documentation.


### [Community](https://jhpce.jhu.edu/knowledge-base/community/)
- [Perl](https://jhpce.jhu.edu/knowledge-base/perl/)
- [Python](https://jhpce.jhu.edu/knowledge-base/python/)
- [R](https://jhpce.jhu.edu/knowledge-base/r/)
- [Short Read Tools](https://jhpce.jhu.edu/knowledge-base/short-read-tools/)

