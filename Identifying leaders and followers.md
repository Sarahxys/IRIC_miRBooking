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

To isolate gene with significant differential expression(1=ID; 24 = log2foldchange; 26=adjusted p-value):
```
awk '$24 > 0.2 && $26 >0 && $26 < 0.05 {print $1, $24, $26;}' HV2R0HYvsHV2D10HY.complete.txt | nl

```


