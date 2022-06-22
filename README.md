# FCOM22

Supplementary materials to the poster presented on the FEMS Conference on Microbiology 2022




## Workflow  
Tools used in the current work:
- FastQC v0.11.9
- MultiQC v1.12.dev0
- bbduk (Last modified October 17, 2017)
- bowtie2
- samtools
- featureCounts
- R version
- R studio version
- eggnog-mapper (LINK)
- DAVID (LINK)
- operon-mapper (LINK)
- DNA Features Viewer (python3 package)
- hmmer 
- 


### Workflow for phylogenetic analysis 

1. Retrieving data: 
 - bacterial complete genome sequences and annotations of latest version from the RefSeq FTP server 
 - id mapping metadata from Uniprot FTP server 
 - Pfam35.0 database
 - taxonomy infromation from ftp://ftp.ncbi.nih.gov/pub/taxonomy/ 

**"phmmer + blastp" approach**:
2. Search for homologs of Yih proteins from _E. coli_ K-12 MG1655 (`phmmer`) in the Pfam protein database (Pfamseq), extracting protein sequences from the Pfamseq (`esl-sfetch`), use them as a local database (`makeblastdb`) for additional analysis of Yih protein query sequences by `blastp` (`-outfmt 7 -evalue 1e-3 -num_descriptions 30000  -num_alignments 30000`), select blastp results with E-value < 0.0
3. Id mapping of protein homologs: cross-reference UniProt Knowledgebase accession numbers (UniProtKB-AC) to GenBank protein IDs based on ID mapping metadata from the FTP UniProt site 
4. Select bacterial homologs according to taxonomy information from the FTP NCBI site
5. For each protein sequence, get location of the respective gene from GFF annotations


**"nsimscan + blastp" approach**:
To increase the number of strains in our dataset with the Yih protein homologs, independently of the procedure described above, we found nucleotide homologs of yih genes and then obtained respective protein sequences.
6. Identify genomic regions homologous to yih genes against bacterial genomes (`nsimscan`, options: `-v -k 8 -t 150 --it 55 --xt 55 --rpq 10000 --om M8 --maxslen 10000000 --minlen 70 --mdom`). 
7.  Output of nucleotide homology search contained information on target GenBank genome IDs and genomic coordinates of homologous fragments. For each homologous region, midpoint genomic location was calculated and used for extracting GenBank protein ID and protein sequence from translated CDS records. From these candidate Yih homologs, a local database was built for BLASTP analysis where Yih proteins of E. coli str. K-12 substr. MG1655 were again used as the query. BLASTP results with E-value < 0.001 were selected.
After that, we pooled protein sequences of Yih homologs obtained from both “phmmer + blastp” and “nsimscan+blastp” techniques into a single dataset (Fig. 11).

### Workflow for RNA-seq analysis 

1. Primary quality check of reads data: `FastQC` and `MultiQC`
2. Trimming and filtering reads: `bbduk` (oprtions: `-Xmx1g ktrim=r k=23 mink=11 hdist=1 tpe tbo`)
3. rRNA decontamination: mapping to rrna genes of respective strain (`bowtie2`), getting unmapped reads (`samtools `), quality check  (`FastQC` + `MultiQC`)
4. Mapping to reference genomes (_E. coli_ K12: GCF_000005845.2, _E. coli_ Nissle 1917: GCF_019967895.1): `bowtie2` + `samtools`
5. Counting reads to genomic features: `featureCounts`
6. Differential expression analysis: `DESeq2`
7. Functional analysis of DE genes:  `clusterProfiler`, `pathview`, `eggnog-mapper` (web-server), `DAVID` (web-server)
8. Operon prediction and visualization: `operon-mapper`, `DNA Features Viewer` (python3)

## References

Denger, K., Weiss, M., Felux, A., Schneider, A., Mayer, C., Spiteller, D., … Schleheck, D. (2014). in the biogeochemical sulphur cycle. Nature, 507(7490), 114–117.https://doi.org/10.1038/nature12947

Kaznadzey, A., Shelyakin, P., Belousova, E., & Eremina, A. (2018). The genes of the sulphoquinovose catabolism in Escherichia coli are also associated with a previously unknown pathway of lactose degradation. Scientific Reports, (February), 1–12. https://doi.org/10.1038/s41598-018-21534-3

Shimada T, Yamamoto K, Nakano M, Watanabe H, Schleheck D, Ishihama A. Regulatory role of CsqR (YihW) in transcription of the genes for catabolism of the anionic sugar sulfoquinovose (SQ) in Escherichia coli K-12. Microbiology (Reading). 2019 Jan;165(1):78-89. doi: 10.1099/mic.0.000740. PMID: 30372406.
