
**RNA-sequencing analysis**

**`DGE_analysis.Rmd`** contains code used for differential gene expression analysis. 

**`Volcano`** plots were generated as a part of differential gene expression analysis (threshold for adjusted p-value is 0.05, cutoff value of Log2FoldChange is either 1.32 (_E. coli_ str. Nissle 1917; growth conditions: lactose growth vs glucose) or 2 (for other cases). 


Heatmap plots **`pheatmap`** diplay a distribution for vst-transformed counts of top 100 variance genes filtered with Log2FoldChange cutoff of 2 and adjusted p-value of 0.05. COG-categories were predicted using eggnog-mapper web service. 
