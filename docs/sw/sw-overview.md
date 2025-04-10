### Introduction

The Joint High Performance Computing Exchange (JHPCE) offers a variety of software for use on the cluster. They are installed and running on Rocky Linux 9.4. We use [Lmod module system](modules.md) to manage the installed software. You can run `module aval` at the cluster prompt for the available software.

### Current available software
```
[test@compute-110 ~]$ module avail

--------------------------------------------------------------------------------- /jhpce/shared/jhpce/modulefiles ---------------------------------------------------------------------------------
   JHPCE_ROCKY9_DEFAULT_ENV (L)    bcl2fastq/2.17.1              freesurfer/7.4.1           gmp/6.2.1                 mathematica/13.3        python/3.11.8             stata/17
   JHPCE_tools/3.0          (L)    bowtie/2.5.1                  fsl/6.0.6.5                go/20.6                   matlab/R2023a    (D)    rstudio/2023.06.1         tex/20240117
   R_test/4.3.2                    conda/3-23.3.1-testing        gcc/9.5.0                  helix/23.10.0             matlab/R2023b           rust/1.76.0               wine/7.11
   afni/23.3.09                    conda/3-23.3.1         (D)    gcc/13.1.0          (D)    java/19            (D)    mpc/1.3.1               sas/9.0
   alphafold/4.3.1                 dcmtk/3.6.7                   gdal/3.6.0                 julia/1.9.2               mpfr/4.2.0              sas/9.4            (D)
   anaconda/2023.03                encfs/1.9.5                   ghostscript/10.02.1        kakoune/2023-08-05        python/3.9.14    (D)    shapeit/5.1.1
   aws/2.12.7                      ffmpeg/6.0                    glpk/5.0                   latex/20240117            python/3.10.13          singularity/3.11.4

------------------------------------------------------------------------------- /jhpce/shared/community/modulefiles -------------------------------------------------------------------------------
   R/4.3    bedtools/2.31.0    conda_R/devel    conda_R/test    conda_R/4.3.x (D)    conda_R/4.3

--------------------------------------------------------------------------------- /jhpce/shared/libd/modulefiles ----------------------------------------------------------------------------------
   PRSice/2.2.13           cell2location/0.1.3          fusion_twas/github            java/17                   plink/1.90b              samtools/1.18       (D)    trimgalore/0.6.6
   Salmon/1.2.1            cell2location/0.8a0   (D)    gatk/4.2.0.0                  java/18                   plink/2.00a4.6    (D)    samui/1.0.0-next.24        trimmomatic/0.39
   Salmon/1.10.1    (D)    cellpose/2.2.2               gffread/github                kallisto/0.46.1           qctool/2.0.7             samui/1.0.0-next.45 (D)    vampire/3.4.4
   arioc/1.51              cellprofiler/4.2.6           gffread/0.12.7         (D)    ldsc/1.0.1                qtl_gtex/dae3369         spaceranger/2.1.0          vcftools/0.1.16
   bamtofastq/1.4.1        cellranger/7.0.0             git-lfs/3.4.0                 magma/1.10                r_nac/1.0                spagcn/1.2.0               wiggletools/1.2.1
   bcftools/1.10.2         cellranger/7.2.0      (D)    git-status-size/github        methyldackel/0.5.2        regtools/0.5.33g         star/2.7.8a                wigtobigwig/2.9
   bcftools/1.18    (D)    cellranger_arc/2.0.2         graphst/da29b75               methylpy/1.4.3            resept/1.0.0             subread/2.0.0
   bfg/1.13.0              cibersortx/04_04_2020        hipstr/0.7                    nextflow/20.01.0          rmate/1.5.10             synapse/2.7.2
   bismark/0.23.0          dissect/dc45940c             hisat2/2.2.1                  nextflow/22.10.7          rseqc/3.0.1              synapse/3.1.1       (D)
   bs/1.3.0                fastqc/0.11.8                htslib/1.10.2                 nextflow/23.10.0   (D)    samblaster/0.1.26        tangram/1.0.4
   bustools/0.39.3         fastqc/0.12.1         (D)    htslib/1.18            (D)    paste/1.3.0               samtools/1.10            tensorqtl/1.0.8
```

### Resources for some software
- [Matlab](matlab.md)
- [Python](python.md)
- [R](r-n-friends.md )
- [SAS](sas.md)
- [Stata](stata.md)
