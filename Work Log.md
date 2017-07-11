# Work log
# Difference in Header between mirun output with HV2_R0HY *
I used a script to compare the header of mirun output files ran with HV2_R0HY cell_line. Theoritically they should be the same since I used the same cell line.
The command that I ran:
```
perl CompareHeader_BC.pl ../HV2_MS_mirun/HV2_ROHY/resultROHY_0and6000_all overexp_noZero_R0HY_mirun/resultNM_030678 overexp_R0HY_mirun/resultNM_030696
```
I got output:
```
number of item in each header: 17764    17763   17764
item that is in file 1 but not 2:
NM_001270484
-------------
item that is in file 1 but not 3:
NM_001270484
-------------
item that is in file 2 but not 1:
-------------
item that is in file 2 but not 3:
------------------
item that is in file 3 but not 1:
NM_030696
------------------
item that is in file 3 but not 2:
NM_030696
```
Problem: why it is different? did the database have been updated?
