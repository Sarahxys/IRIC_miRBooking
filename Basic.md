# Basic Commands for miRBooking and others
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
