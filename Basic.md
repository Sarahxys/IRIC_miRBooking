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
