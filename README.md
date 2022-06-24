# Multiple functions and forms of the _Escherichia coli yih_ cassette

Supplementary materials to the poster presented on the FEMS Conference on Microbiology 2022 (FCOM22)

## Abstract
**Background**: The _ompLyihOPQRSTUVW_ cassette (further, the _yih_ cassette) of _Escherichia coli_ K-12 MG1655 participates in the degradation of sulfoquinovose (SQ), a derivative of glucose. Recent results in our laboratory demonstrated the possible connection of several _yih_ genes with lactose utilisation. We intend to examine the role of _yih_ genes in an alternative pathway of lactose catabolism in more detail and analyse the evolution of the _yih_ cassette. 

**Objectives**: We aim to study the evolution of the _yih_ cassette and explore the possibility of its multifunctionality.

**Methods**: The phylogenetic analysis allowed us to determine patterns of the _yih_ genes co-localization. RNA-sequencing and qRT-PCR were performed to assess the expression of _yih_ genes in _E. coli_ str. K-12 MG1655 and _E. coli_ str. Nissle 1917 strains during growth on glucose (a baseline condition), lactose, and SQ. 

**Results**: According to the phylogenetic analysis, the _yih_ cassette exists mainly in the Enterobacteriales order as one of two forms, long (e.g., _ompLyihOPQRSTUVW_ in _E. coli_ str. K-12 MG1655) or short (e.g., _yihTUVW_ in _E. coli_ str. Nissle 1917). Our qRT-PCR results demonstrated a distinct activation of the _yih_ genes of the long cassette during growth on sulfoquinovose and a slight activation on lactose. In contrast, _yih_ genes of the short cassette did not respond to the differences in these carbon sources. Differential expression analysis results were consistent with these observations. We suggest that the long _yih_ cassette is responsible for the SQ and lactose degradation. The short variant might be involved in other metabolic pathways.


## Workflow  
Tools used in the current work:
- FastQC v0.11.9
- MultiQC v1.12.dev0
- bbduk (Last modified October 17, 2017)
- bowtie2 v2.2.1
- samtools v1.11
- featureCounts v2.0.1
- R version 3.6.3
- R studio version 1.4.1717
- [eggnog-mapper](http://eggnog-mapper.embl.de)
- [DAVID](https://david.ncifcrf.gov/tools.jsp) 
- [operon-mapper](https://biocomputo.ibt.unam.mx/operon_mapper/)
- DNA Features Viewer, biopython, numpy, pandas, matplotlib, seaborn (python3 packages)
- phmmer v3.1b2
- esl-sfetch vh3.1b2
- BLAST 2.9.0+
- nsimscan v1.1.84
- mafft v7.475
- FastTree v2.1.11 No SSE3

### Workflow for phylogenetic analysis 

**0. Retrieve data**: 
 - bacterial complete genome sequences and annotations of latest version, respective translated CDS records (from the RefSeq FTP server);  
 - id mapping metadata (from Uniprot FTP server);
 - Pfam35.0 database;
 - taxonomy infromation (from ftp://ftp.ncbi.nih.gov/pub/taxonomy/) 


**1.1. Search for homologs of Yih proteins: "phmmer + blastp" approach**

- Search for homologs of Yih proteins from _E. coli_ K-12 MG1655 (`phmmer`) in the Pfam protein database (Pfamseq), extracting protein sequences from the Pfamseq (`esl-sfetch`), use them as a local database (`makeblastdb`) for additional analysis of Yih protein query sequences by `blastp` (`-outfmt 7 -evalue 1e-3 -num_descriptions 30000  -num_alignments 30000`), select BLASTP results with E-value < 0.0
- Id mapping of protein homologs: cross-reference UniProt Knowledgebase accession numbers (UniProtKB-AC) to GenBank protein IDs based on ID mapping metadata from the FTP UniProt site (_ad hoc_ bash scripts)
- Select bacterial homologs according to taxonomy information from the FTP NCBI site (_ad hoc_ bash scripts)
- For each protein sequence, get location of the respective gene from GFF annotations (_ad hoc_ python scripts)


**1.2. Search for homologs of Yih proteins: "nsimscan + blastp" approach**:

To increase the number of strains in our dataset with the Yih protein homologs, independently of the procedure described above, we found nucleotide homologs of yih genes and then obtained respective protein sequences.

- Identify genomic regions homologous to yih genes against bacterial genomes `nsimscan` (`-v -k 8 -t 150 --it 55 --xt 55 --rpq 30000 --om M8 --maxslen 10000000 --minlen 70 --mdom`). 
- Calculate midpoint genomic location for each homologous region (_ad hoc_ python scripts)
- Extract protein sequence of candidate Yih homologs from translated CDS records (_ad hoc_ python scripts)
- Build a local database (`makeblastdb`) and run `blastp`using Yih proteins of _E. coli_ str. K-12 substr. MG1655 as the query; select BLASTP results with E-value < 0.001

Pool protein sequences of Yih homologs obtained from both "phmmer + blastp" and "nsimscan+blastp" approaches into a single dataset (_ad hoc_ python scripts).


**2. Identify cassette content**

Two _yih_ gene homologs were considered as co-localized if the distance between their middle coordinates on the genome was less than 10 000 nucleotides 
- Retrieve coordinates of genes coding for homologs of Yih proteins from GFF annotation files (_ad hoc_ python scripts) 
- Put GenBank genome ID and organism name in concordance with mutual positioning of _yih_ homologs on the chromosomes (_ad hoc_ python scripts) 

**3. Construct protein phylogenetic trees**

According to Kaznadzey et al. (2018), growth of the _E. coli_ culture on lactose as a sole carbon source resulted in the significantly enhanced expression of four yih genes, notably _yihS_, _yihT_, _yihV_, _yihW_. Therefore, we further refer to these genes as “core” genes, presumably having multifunctional characteristics. Assessing protein phylogenetic trees, we selected organisms that possessed a cassette consisting of at least three core _yih_ genes and subjected them to subsequent analysis.

- Protein multiple sequence alignment: `mafft` (default options)
- Compute approximately-maximum-likelihood phylogenetic trees: `FastTree` (default options)
- Visualize phylogenetic trees: `Itol` (web server); annotation files for ITOL were generated using _ad hoc_ developed python scripts


### Workflow for _E. coli_ Nissle 1917 genome assembly 

The raw reads have been deposited in the NCBI SRA under the accession no. SRR14244429.

The whole-genome shotgun project has been deposited in DDBJ/ENA/GenBank under the accession no. PRJNA722272.



### Workflow for RNA-seq analysis 

1. Primary quality check of reads data: `FastQC` and `MultiQC`
2. Trimming and filtering reads: `bbduk` (options: `-Xmx1g ktrim=r k=23 mink=11 hdist=1 tpe tbo`)
3. rRNA decontamination: mapping to rrna genes of respective strain (`bowtie2`); converting SAM to BAM, sorting, indexing (`samtools`); getting unmapped reads (`samtools view -b -f 4`); sorting by name (`samtools sort -n`), fixing mate pairs/info (`samtools fixmate`), converting to mate-fixed bam to fastq (`bedtools bamtofastq -i`); quality check (`FastQC` + `MultiQC`)
4. Mapping to reference genomes (_E. coli_ K12: GCF_000005845.2, _E. coli_ Nissle 1917: GCF_019967895.1): `bowtie2` (mapping options: `--local -x`) + `samtools`
5. Counting reads to genomic features: `featureCounts` (options: `-p -F -f -t gene --extraAttributes db_xref,gene  -T 5 -o`, input annotation in gtf format)
6. Differential expression analysis: `DESeq2`
7. Functional analysis of DE genes:  `clusterProfiler`, `pathview`, `eggnog-mapper` (web-server), `DAVID` (web-server)
8. Operon prediction and visualization: `operon-mapper`, `DNA Features Viewer` (python3)

## Authors

- Anna Rybina, Skolkovo Institute of Science and Technology
- Artemiy Dakhnovets, Lomonosov Moscow State University, Institute of Cell Biophysics RAS 
- Anna Kaznadzey, Institute for Information Transmission Problems RAS
- Maria Tutukina, Skolkovo Institute of Science and Technology, Institute for Information Transmission Problems RAS
- Mikhail Gelfand, Skolkovo Institute of Science and Technology, Institute for Information Transmission Problems RAS


## References

Denger, K., Weiss, M., Felux, A., Schneider, A., Mayer, C., Spiteller, D., … Schleheck, D. (2014). in the biogeochemical sulphur cycle. Nature, 507(7490), 114–117.https://doi.org/10.1038/nature12947

Kaznadzey, A., Shelyakin, P., Belousova, E., & Eremina, A. (2018). The genes of the sulphoquinovose catabolism in Escherichia coli are also associated with a previously unknown pathway of lactose degradation. Scientific Reports, (February), 1–12. https://doi.org/10.1038/s41598-018-21534-3

Shimada T, Yamamoto K, Nakano M, Watanabe H, Schleheck D, Ishihama A. Regulatory role of CsqR (YihW) in transcription of the genes for catabolism of the anionic sugar sulfoquinovose (SQ) in Escherichia coli K-12. Microbiology (Reading). 2019 Jan;165(1):78-89. doi: 10.1099/mic.0.000740. PMID: 30372406.
