# FCOM22

Supplementary materials to the poster presented on the FEMS Conference on Microbiology 2022




## Workflow  

Tools used in the current work:
* FastQC v0.11.9
* MultiQC v1.12.dev0
* bbduk (Last modified October 17, 2017)
* bowtie2
* samtools
* featureCounts
* R version
* R studio version
* eggnog-mapper (LINK)
* DAVID (LINK)
* operon-mapper (LINK)
* DNA Features Viewer (python3 package)

### Workflow for RNA-seq analysis 

1. Primary quality check of reads data: `FastQC` and `MultiQC`
2. Trimming and filtering reads: `bbduk` (oprtions: `-Xmx1g ktrim=r k=23 mink=11 hdist=1 tpe tbo`)
3. rRNA decontamination: mapping to rrna genes (bowtie2), getting unmapped reads (samtools), quality check  (fastqc + MultiQC)
4. Mapping to reference genomes (_E. coli_ K12: GCF_000005845.2, _E. coli_ Nissle 1917: GCF_019967895.1): bowtie2 + samtools
5. Counting reads to genomic features: `featureCounts`
6. Differential expression analysis: `DESeq2`
7. Functional analysis of DE genes:  `clusterProfiler`, `pathview`, `eggnog-mapper` (web-server), `DAVID` (web-server)
8. Operon prediction and visualization: `operon-mapper`, `DNA Features Viewer` (python3)

## References

Denger, K., Weiss, M., Felux, A., Schneider, A., Mayer, C., Spiteller, D., … Schleheck, D. (2014). in the biogeochemical sulphur cycle. Nature, 507(7490), 114–117.https://doi.org/10.1038/nature12947

Kaznadzey, A., Shelyakin, P., Belousova, E., & Eremina, A. (2018). The genes of the sulphoquinovose catabolism in Escherichia coli are also associated with a previously unknown pathway of lactose degradation. Scientific Reports, (February), 1–12. https://doi.org/10.1038/s41598-018-21534-3

Shimada T, Yamamoto K, Nakano M, Watanabe H, Schleheck D, Ishihama A. Regulatory role of CsqR (YihW) in transcription of the genes for catabolism of the anionic sugar sulfoquinovose (SQ) in Escherichia coli K-12. Microbiology (Reading). 2019 Jan;165(1):78-89. doi: 10.1099/mic.0.000740. PMID: 30372406.
