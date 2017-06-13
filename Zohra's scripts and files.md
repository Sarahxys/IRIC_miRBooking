This page is used to document the steps that I should follow to use Zohra's scripts and files with the optimal goal of identifying leaders and followers. 

all the data files; 
```
 /u/zsaci/TCGA_raw_counts/tables/ 
 ```
 
 filter p-value and cor; can modify # for p-value and correlation
 ```
 perl ~/projet_Sylvie_Mader/new_data/communs/bons_noms/correlation/filtre_independat.pl resultat1_1_corr > filtre_correlation_1_1
head -1 resultat1_1_corr > header

cat header filtre_correlation_1_1 > header_filtre_correlation_1_1

perl ~/projet_Sylvie_Mader/new_data/communs/bons_noms/correlation/filtre_correlation.pl resultat_corr_1_1 resultat_pval_1_1 > filtre_corr_pval_1_1
```

R script to filter out 0
```
filtre_corr1_1_pval=read.table("filtre_corr_pval_1_1",  as.is=T,sep="\t",quote="\"'", dec=".", header=TRUE, row.names=1)
filtre<-filtre_corr1_1_pval[colSums(filtre_corr1_1_pval!=0)!=0]
filtre_matrix=data.matrix(filtre)

library("gplots")
library(RColorBrewer)
coule<-brewer.pal(11, "Spectral")[11:1]
piwi<-colorRampPalette(coule)
png("filtre1_1_expression_silencing.png", 1200,1200)
heat_filtre<-heatmap.2(filtre_matrix, col=piwi(75), scale="none",  key=TRUE, symkey=FALSE, trace="none", dendrogram="none")
dev.off()

write.table(filtre, file="corrige_corr_filtre1_1", quote=FALSE, sep="\t", eol = "\n", dec = ".", row.names = TRUE, col.names = TRUE)
```


 
