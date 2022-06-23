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

0. Retrieving data: 
 - bacterial complete genome sequences and annotations of latest version, respective translated CDS records (from the RefSeq FTP server);  
 - id mapping metadata (from Uniprot FTP server);
 - Pfam35.0 database;
 - taxonomy infromation (from ftp://ftp.ncbi.nih.gov/pub/taxonomy/) 


1.1. **Search for homologs of Yih proteins: "phmmer + blastp" approach**

- Search for homologs of Yih proteins from _E. coli_ K-12 MG1655 (`phmmer`) in the Pfam protein database (Pfamseq), extracting protein sequences from the Pfamseq (`esl-sfetch`), use them as a local database (`makeblastdb`) for additional analysis of Yih protein query sequences by `blastp` (`-outfmt 7 -evalue 1e-3 -num_descriptions 30000  -num_alignments 30000`), select BLASTP results with E-value < 0.0
- Id mapping of protein homologs: cross-reference UniProt Knowledgebase accession numbers (UniProtKB-AC) to GenBank protein IDs based on ID mapping metadata from the FTP UniProt site 
- Select bacterial homologs according to taxonomy information from the FTP NCBI site
- For each protein sequence, get location of the respective gene from GFF annotations


1.2. **Search for homologs of Yih proteins: "nsimscan + blastp" approach**:

To increase the number of strains in our dataset with the Yih protein homologs, independently of the procedure described above, we found nucleotide homologs of yih genes and then obtained respective protein sequences.

- Identify genomic regions homologous to yih genes against bacterial genomes `nsimscan` (`-v -k 8 -t 150 --it 55 --xt 55 --rpq 30000 --om M8 --maxslen 10000000 --minlen 70 --mdom`). 
- Calculate midpoint genomic location for each homologous region
- Extract protein sequence of candidate Yih homologs from translated CDS records 
- Build a local database (`makeblastdb`) and run `blastp`using Yih proteins of _E. coli_ str. K-12 substr. MG1655 as the query; select BLASTP results with E-value < 0.001

Pool protein sequences of Yih homologs obtained from both "phmmer + blastp" and "nsimscan+blastp" approaches into a single dataset


2. **Identification of cassette content**
First, coordinates of genes coding for homologs of Yih proteins were retrieved from GBFF annotation data. As was mentioned before, two yih gene homologs were considered as co-localized if the distance between their middle coordinates on the genome was less than 10 000 nucleotides (Fig. 11). To put GenBank genome ID and organism name in concordance with mutual positioning of yih homologs on the chromosomes and to study the obtained cassette combinations, ad hoc python scripts were written.

3. **Construction of protein phylogenetic trees**
Protein sequences of Yih homologs were aligned using the MUSCLE tool (Edgar et al., 2004). If multiple sequence alignment consisted of less than 500 sequences, protein phylogenetic trees were built by Maximum likelihood algorithm using PhyML program (Guindon et al., 2003). Otherwise, FastTree was applied (Price et al., 2010). MUSCLE, PhyML and FastTree were run with default parameter. Phylogenetic trees were displayed using a web server ITOL (Letunic and Bork, 2006). To add details to tree, visualization annotation files for ITOL were generated using ad hoc developed python scripts.

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
