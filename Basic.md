# Basic Commands for miRBooking and others

# miRBooking
to run miRBooking:
```
java -Xmx10g -jar mirdesign_0.16.jar -s scores -i test_EMT.mirun --output result -e experiments --experiment-format advanced
```

# MySQL
to access mysql
```
mysql --host binsrv2.iric.ca mirdesign
```
to get the table; the second one is the actually table and it is pasted here for easier assess for myself:
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

to sesearch for particular information (tutorial: https://dev.mysql.com/doc/refman/8.0/en/selecting-rows.html):
```
select * from cell_line where name like '%...%'
```
formats of each database:
- cell line
  ```
  | id   | name          | version | dataset | species      | source   | notes                    
  ```
- mirna
  ```
  accession    | name                 | sequence                           | seed_index |
  ```
 




# VIM
to edit
```
vi test_EMT.mirun
```
to exit
```
:q
```
to overide/save
```
:qw
```
to insert/change
```
:i
```

#Rotate.R script
to run this script
```
Rscript rotate.R result
```
