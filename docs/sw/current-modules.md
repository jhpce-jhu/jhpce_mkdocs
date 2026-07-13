## Currently Install Module on JHPCE and JADE
```console

------------------------------------- /jhpce/shared/jhpce/modulefiles -------------------------------------
   DMFold/1.2                      gcc/13.1.0          (D)    ollama/0.30.8
   JADE_ROCKY9_DEFAULT_ENV         gdal/3.6.0                 python/3.9.14      (D)
   JHPCE_ROCKY9_DEFAULT_ENV (L)    gh/2.46.0                  python/3.10.13
   JHPCE_tools/3.0          (L)    ghostscript/10.02.1        python/3.11.8
   R_test/4.3.2                    glpk/5.0                   python/3.12.12
   afni/23.3.09                    gmp/6.2.1                  python/3.13.4
   alphafold/2.3.1          (D)    go/20.6             (D)    python/3.14.6
   alphafold/3.0.0                 go/22.1                    python2/2.7.9      (D)
   anaconda/2023.03         (D)    go/23.1                    python2/2.7.18
   anaconda/2024.10                go/26.1                    rclone/1.69.1
   aws/2.12.7                      gurobi/11.0.3              reportseff/2.3.2
   azcopy/10.24.0                  gurobi/12.0.3       (D)    rstudio/2023.06.1
   bcl2fastq/2.17.1                helix/23.10.0              rstudio/2024.04.2  (D)
   blast/2.16.0                    java/19             (D)    rstudio/2025.05.1
   bowtie/2.5.1                    jhuacg/14.0.1              ruby/3.1.2
   code-server/4.112.0             jrt-test                   rust/1.76.0
   conda/3-23.3.1-testing          julia/1.9.2         (D)    sas/9.0
   conda/3-23.3.1                  julia/1.10.5               sas/9.4            (D)
   conda/3-24.3.0           (D)    julia/1.11.1               shapeit/5.1.1
   cuda/12.1.1                     kakoune/2023-08-05         singularity/3.11.4
   dcmtk/3.6.7                     latex/20240117             sra-toolkit/3.1.1
   encfs/1.9.5                     matlab/R2023a              stata/17
   ffmpeg/6.0                      matlab/R2023b              tesseract/5.5.1
   freesurfer/7.4.1                matlab/R2025a       (D)    tex/20240117
   fresh/0.1.55                    mpc/1.3.1                  texstudio/4.8.7
   fsl/6.0.6.5                     mpfr/4.2.0                 wine/7.11
   gcc/9.5.0                       node/24.11

----------------------------------- /jhpce/shared/community/modulefiles -----------------------------------
   R/4.3              conda_R/devel    conda_R/4.3.x    conda_R/4.4.x        conda_R/4.5.x
   bedtools/2.31.0    conda_R/test     conda_R/4.3      conda_R/4.4   (D)    conda_R/4.5

------------------------------------- /jhpce/shared/libd/modulefiles --------------------------------------
   PRSice/2.2.13                 graphst/da29b75           ruby/3.2.2          (D)
   Salmon/1.2.1                  harmony2/2.0.1            rust/1.95.0         (D)
   Salmon/1.10.1          (D)    hergast/0.0.1             samblaster/0.1.26
   arioc/1.52                    hipstr/0.7                samtools/1.10
   aws/2.28               (D)    hisat2/2.2.1              samtools/1.18       (D)
   bamtofastq/1.4.1              htslib/1.10.2             samui/1.0.0-next.24
   bcftools/1.10.2               htslib/1.18        (D)    samui/1.0.0-next.45
   bcftools/1.18          (D)    java/17                   samui/1.0.0-next.49
   bfg/1.13.0                    java/18                   samui/1.0.1         (D)
   bin2cell/0.3.0                kallisto/0.46.1           spaceranger/2.1.0
   bismark/0.23.0                ldsc/1.0.1                spaceranger/3.0.0
   bs/1.3.0                      leafcutter/0.2.9          spaceranger/3.1.1
   bustools/0.39.3               liana_plus/1.5.1          spaceranger/3.1.2
   cell2location/0.1.3           liana_plus/1.7.1   (D)    spaceranger/3.1.3
   cell2location/0.8a0    (D)    liftover/1.0              spaceranger/4.0.1
   cellpose/2.0                  magma/1.10                spaceranger/4.1.0   (D)
   cellpose/2.2.2         (D)    methyldackel/0.5.2        spagcn/1.2.0
   cellprofiler/4.2.6            methylpy/1.4.3            spatula/f0e9936
   cellranger-atac/2.1.0         nda-tools/0.2.27          spatula/1.0.0
   cellranger/7.0.0              nda-tools/0.3.0           spatula/7fe7171     (D)
   cellranger/7.2.0              nda-tools/0.5.0           stalign/1.0.1
   cellranger/8.0.1              nda-tools/0.6.0           star/2.7.8a
   cellranger/9.0.0              nda-tools/0.7.0    (D)    subread/2.0.0
   cellranger/9.0.1              nextflow/20.01.0          synapse/2.7.2
   cellranger/10.0.0      (D)    nextflow/22.10.7          synapse/3.1.1       (D)
   cellranger_arc/2.0.2          nextflow/23.10.0          tangram/1.0.4
   cibersortx/04_04_2020         nextflow/24.10.5   (D)    tensorqtl/1.0.8
   dissect/dc45940c              paste/1.3.0               trimgalore/0.6.6
   fastqc/0.11.8                 plink/1.90b               trimmomatic/0.39
   fastqc/0.12.1          (D)    plink/2.00a4.6     (D)    vampire/3.4.4
   ficture/dev_a455e5c           plink2/2.0                vcftools/0.1.16
   ficture/0.0.3.1        (D)    qctool/2.2.5              visium_hd/1.0
   fusion_twas/github            qtl_gtex/dae3369          wiggletools/1.2.1
   gatk/4.5.0.0                  r_nac/1.0                 wigtobigwig/2.9
   gffread/github                regtools/0.5.33g          xeniumranger/1.7.1
   gffread/0.12.7         (D)    resept/1.0.0              xeniumranger/2.0.0
   git-lfs/3.4.0                 rmate/1.5.10              xeniumranger/3.1.1
   git-status-size/github        rseqc/3.0.1               xeniumranger/4.0.0  (D)

  Where:
   D:  Default Module
   L:  Module is loaded

If the avail list is too long consider trying:

"module --default avail" or "ml -d av" to just list the default modules.
"module overview" or "ml ov" to display the number of modules for each name.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".


```
Note: some modules may not be available on the login node or on the JADE nodes.
