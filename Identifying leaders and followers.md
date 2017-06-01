# Introduction
This page will document the workflow that I used to identify leader and followers. As for now, it is just some thoughts. It will probably change depends on how Zohra's scripts work.

# Runing miRBooking
For identifying followers of a gene: 
- The script that I used to run miRBooking will modify the expression level of the target/gene of interests (typically 100-6000 with increment of 100). 

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

- A output file 'result' will be rotated to make it readable. This will generate file 'resultrt'. 

# Isolating Info 
To grep the silencing power for gene/target of interested (ex. NM_010699) from resultrt: 
```
awk 'NR==1 || /NM_010699/' result1000rt >isolated1000
```


