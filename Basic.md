# Introduction
This page is created to store basic commands for miRBooking and others that I use frequently. 

# miRBooking
To run miRBooking:
```
java -Xmx10g -jar ~/mirdesign_0.16.jar -s ~/scores -i HV2_LTHY1.mirun --output result -e experiments --experiment-format advanced
```
To run miRBooking with a different database
```
java -jar mirdesign_0.16.jar --input input.md --database binsrv2/mirdesign_hg38 --experiment-output experiments --experiment-format advanced

```

# MySQL
To access mysql
```
mysql --host binsrv2.iric.ca mirdesign
```
To get the table; the second one is the actually table and it is pasted here for easier assess for myself:
```
show tables;
```
```
+---------------------+
| Tables_in_mirdesign |
+---------------------+
| cell_line           |
| mirna               |
| mirna_expression    |
| mirna_to_remove     |
| target              |
| target_expression   |
+---------------------+

```

To sesearch for particular information (tutorial: https://dev.mysql.com/doc/refman/8.0/en/selecting-rows.html):
```
select * from cell_line where name like '%...%'
```
Formats of each database:
- cell line
  ```
  | id   | name          | version | dataset | species      | source   | notes                    
  ```
- mirna
  ```
  accession    | name                 | sequence                           | seed_index |
  ```
- mirna_expression
  ```
  | cell_line | accession    | quantity |
  ```
- target
  ```
  | accession    | name | variant | cds_start | cds_end | sequence   
  ```
- target_expression
  ```
  | cell_line | accession | quantity | 
  ```




# VIM
To create vim file or to edit
```
vi test_EMT.mirun
```
To exit
```
:q
```
To overide/save
```
:wq
```
To insert/change
```
:i
```

# Rotate.R script
to run this script
```
Rscript ~/rotate.R result
```

# Access folders
first author 
```
cd weilln
```

# Change permission to enable edit
```
chmod 777
```
# Using the python version
to run the mirbooking
```
time mirbooking --targets GCF_000001635.25_GRCm38.p5_rna.fna --cds-regions cds-regions.tsv --mirnas mature.fa --score-table scores --quantities HV2_MS_Jordan/AvExpLevel/moyenne_HV2_D10HY
```
to aggregate/summarize result by mirna locaiton:
```
cat results | mirbooking -aggregate mirna location |less 
```
to aggregate/summerize result by target:
```
cat results | mirbooking -aggregate mirna location |less 
```

# Submit jobs using torque
delete all the jobs that was submit by me:
```
qselect -u songs | xargs qdel
```

# Accessing jupyter notebook
```
ssh -L 8889:localhost:8889 songs@cluster.iric.ca
module load python/3.4.4
```

# bash script: rename file extension
```
for _dir in *"leaderfollower"*; do  cd /u/songs/HV2_MS_Jordan/overexp_0to6000/R0HY_overexp_0to6000/identifyLF_cor_moyenneexp/$_dir && for file in *.txt; do  mv "$file" "${file%.txt}.tsv";done;done
```

# Bash script: move a bunch of files from one folder to a different folder
```
for dir in *"leaderfollower_of_NM_"*; do cp /u/songs/HV2_MS_Jordan/overexp_0to6000/R0HY_overexp_0to6000/identifyLF_cor_calibratedexp_filteredbypval/$dir/*noZeroNM* /u/songs/HV2_MS_Jordan/overexp_0to6000/R0HY_overexp_0to6000/; done
```
