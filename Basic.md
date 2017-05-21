# Basic Commands for miRBooking and others

# miRBooking
To run miRBooking:
```
java -Xmx10g -jar mirdesign_0.16.jar -s scores -i test_EMT.mirun --output result -e experiments --experiment-format advanced
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
:qw
```
To insert/change
```
:i
```

#Rotate.R script
to run this script
```
Rscript rotate.R result
```
