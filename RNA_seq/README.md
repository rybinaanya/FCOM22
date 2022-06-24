
**RNA-sequencing analysis**

**`DGE_analysis.Rmd`** contains code used for differential gene expression analysis. 

**`Volcano`** plots were generated as a part of differential gene expression analysis (threshold for adjusted p-value is 0.05, cutoff value of Log2FoldChange is either 1.32 (_E. coli_ str. Nissle 1917; growth conditions: lactose growth vs glucose) or 2 (for other cases). The _yih_ genes of the long variant in _E. coli_ K-12 were activated during growth on SQ. In _E. coli_ Nissle 1917, _yih_ genes did not change their expession upon SQ growth conditions. 


Heatmap plots **`pheatmap`** diplay a distribution for vst-transformed counts of top 100 variance genes filtered with Log2FoldChange cutoff of 2 and adjusted p-value of 0.05. COG-categories were predicted using eggnog-mapper web service. In _E. coli_ K-12, during growth on SQ genes involved in the amino acid and lipid biosynthesis processes were down-regulated and genes linked with sulfoglycolysis, catabolism of fatty acids, amino acids, and carbohydrates were activated. In _E. coli_ Nissle 1917, in the presence of SQ in growth medium, genes connected with amino acid and lipid biosynthesis processes, motility, and chemotaxis were inhibited and genes coding for sugar transporters, catabolism of fatty acids, amino acids, and carbohydrates were up-regulated. As expected, in both strains, lactose and galactose catabolism genes were up-regulated during growth on lactose. 
