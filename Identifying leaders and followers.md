# Introduction
This page will document the workflow that I used to identify leader and followers. As for now, it is just some thoughts. It will probably change depends on how Zohra's scripts work.

# Runing miRBooking
For identifying followers of a gene: 
- The script that I used to run miRBooking will modify the expression level of the target/gene of interests (typically 100-6000 with increment of 100). 
    ```
    define set ELEVELS (100, 200, 300, 400, 500, 600, 700, 800, 900, 1000, 1100, 1200, 1300, 1400, 1500, 1600, 1700, 1800, 1900, 2000, 2100, 2200, 2300, 2400, 2500, 2600, 2700, 2800, 2900, 3000, 3100, 3200, 3300, 3400, 3500, 3600, 3700, 3800, 3900, 4000, 4100, 4200, 4300, 4400, 4500, 4600, 4700, 4800, 4900, 5000, 5100, 5200, 5300, 5400, 5500, 5600, 5700, 5800, 5900, 6000)

  define protocol HY-Ldha use HV2_D10HY(
          NM_010699 = L
  )

  foreach L in ELEVELS (
          test HY-Ldha
  )

    ```

For identifying leaders:
- The script that I used to run miRBooking will set the expression level of all the target/gene to 0 or 6000 (to not-expression or to over-express). The change in silencing powers of the interested gene under each experimental condition will be examined. 
  ```
    define set TARGETS ()
    
    define set QUANTITY (0, 6000)

    define protocol HY-Ldha use HV2_D10HY(
            T = Q
    )

    test HV2_D10HY

    foreach T in TARGETS (
            foreach Q in QUANTITY (
                    test HY-Ldha
            )
    )
  
  ```
 - The run is too memory consuming; need torque to specify a longer run time and enable the allocation of larger memory. Below is the path to the examples:
 ```
 /u/feghalya/miRDesign/HV2-MHY_Jordan/synthDesign
 ```

- A output file 'result' will be rotated to make it readable. This will generate file 'resultrt'. 

# Isolating Info 
To grep the silencing power for gene/target of interested (ex. NM_010699) from 'resultrt': 
```
awk 'NR==1 || /NM_010699/' result1000rt >isolated1000
```
# Differential Expression 
Differential Expression was done with DESeq by Albert and Jordan. From the DESeq output ```HV2R0HYvsHV2D10HY.complete.txt```(format: 1=ID; 24 = log2foldchange; 26=adjusted p-value. path ```/u/songs/HV2_MS_Jordan/DifferentialExpression_HY_total```), I isolated genes with significant differential expression (cutoffs: log2foldchange  ):
```
awk '$24 > 0.2 && $26 >0 && $26 < 0.05 {print $1, $24, $26;}' HV2R0HYvsHV2D10HY.complete.txt  > dfexpr_up_log2fc02_p005
awk '$24 > 0.2 && $26 >0 && $26 < 0.05 {print $1, $24, $26;}' HV2R0HYvsHV2D10HY.complete.txt > dfexpr_down_log2fc01_p005
awk'$24 > 0.2 && $26 < 0.05 {print $1, $24, $26;}' HV2D10HYvsHV2D10TC.complete.txt  > ../../AvExpLevel/overexp_D10HY_mirun/dfexpr_up_log2fc02_p005
awk '$24 < -0.2 && $26 < 0.05 {print $1, $24, $26;}' HV2D10HYvsHV2D10TC.complete.txt > ../../AvExpLevel/overexp_D10HY_mirun/dfexpr_down_log2fc02_p005
awk '$24 > 0.6 && $26 < 0.05 {print $1, $24, $26;}' HV2R0HYvsHV2D10TC.complete.txt > ../../AvExpLevel/overexp_R0HY_mirun/dfexpr_UP_log2fc02_p005
awk '$24 < -0.6 && $26 < 0.05 {print $1, $24, $26;}' HV2R0HYvsHV2D10TC.complete.txt > ../../AvExpLevel/overexp_R0HY_mirun/dfexpr_down_log2fc02_p005
```
Since only gene names were included in the ID(ex, gene1013_Agfg1), a perl script ```get_accession.pl ```(path: ```/u/songs/HV2_MS_Jordan/AvExpLevel```) were ran to extract gene accessions from ```combined_average_expression_updated2.txt``` (path: ```/u/songs/HV2_MS_Jordan/test```): 
```
perl get_accession.pl dfexpr_up_log2fc02_p005 combined_average_expression_updated2.txt
perl get_accession.pl dfexpr_down_log2fc01_p005 combined_average_expression_updated2.txt
```
The above step will generate two seperate output files with accessions (Note: some gene might corresponded to more than ), :


# Python script to cat all the computed correlation result
```
import pandas as pd
from io import StringIO

dfs = []
for i in range(1, 26):
    with open('HV2_MS_Jordan/AvExpLevel/overexp_noZero_R0HY_mirun/cor_subset{}.tsv'.format(i)) as f:
        for line in f:
            dfs.append(pd.read_csv(StringIO(line + next(f)), sep='\t'))

df = pd.concat(dfs)

print(df.to_csv('cor_combined.tsv', sep='\t', index=False, header=True))
```
# Pythone script to compute p-value for the above correlation table using zscore
The below script used zscore to compute p-value; when r= 1 or -1, it indicate perfect correlation and it gave us errors when calculating 1-r^2 (which will equal to zero and cause the zscore to equal to infinity); hence, we made it equal to 0. 
```
import math
from scipy.stats import t

def zscore(r):
    if math.isnan(r):
        return r
    if r == 1 or r == -1:
        return 0
    return 2 * t.cdf(r * math.sqrt(52.0 / (1 - math.pow(r, 2))), df=52.0)

pvalues = df[df.columns[2:]].applymap(zscore)

df[df.columns[2:]] = pvalues

print (df.to_csv('p_val_R0HY.tsv', sep = '\t', index = False, header = True))

```
To run this script: ```Python3 p_val_print.py```. This took 12min to finish running. 

# Python script for generating plot
```
import pandas as pd
df = pd.read_csv('cor_R0HY.tsv', sep='\t', index_col = (0,))
p_df = pd.read_csv('p_val_R0HY.tsv', sep='\t', index_col = (0,))
import matplotlib
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('ggplot')
plt.imshow(df.dropna(thresh=1800).transpose().dropna(thresh=550).transpose().where(p_df <= 0.05, other=0).transpose(), cmap='hot', interpolation='nearest', aspect='equal')
plt.colorbar()
plt.show()
```
# Python script for dropna
```
import pandas as pd
d_drop = d.drop('Expression_Level', axis=1)
p_d_drop = p_d.drop('Expression_Level', axis=1)
d = pd.read_csv('cor_R0HY.tsv', sep='\t', index_col=(0,))
p_d = pd.read_csv('p_val_R0HY.tsv', sep='\t', index_col = (0,))
df_rt = d_drop.dropna(thresh=500).transpose().dropna(thresh=50).where(p_d_drop <= 0.05, other = 0)
print (df_rt.to_csv('filtered_cor_pval_R0HY_dropna.tsv', sep = '\t', index = True, header = True))
```

# Not sure
```
grep NM_010699 filtered_cor_pval_R0HY_rt.tsv > ldha_exp

awk '$2 < -0.95 {print $1, $2;}' ldha_exprt > pfollowers

perl list_diffAcession.pl pfollowers pleaders

mv list_AcessionDiff.txt pfollowers_filtered

perl get_mis.pl pfollowers_filtered resultNM_010699rt > pfollowers_mis

awk '($2-$55)/$2 > 0.5 {print $1, $2,$55}' pfollwers_mis > pfollowers_mis_filtered

```

# Remove extra mRNA accession in the header
Sometimes the headers might have an extra items (being the expressed mRNA itself). This is because the accession might not 
```
ls *resultNM* > listofresult
perl CompareHeader_result.pl listofresult > havediffheader.txt
perl dropextraline.pl havediffheader.txt
module load python/3.4.4
python3 dropextraline.py
```


