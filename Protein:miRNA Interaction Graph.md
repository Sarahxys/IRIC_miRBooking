# Introduction
This documentation was based on the protocal that I ran with Jordan's hypoxia + metabolic stress experimental data. 

# Initial input file 
It will need average expression files before the calibration function.
The names of average expression files are: moyenne_HV2_D10HY, moyenne_HV2_D10TC, moyenne_HV2_R0HY

# Running miRBooking
Run miRBooking with the data from the database.
A short script contain the name of the testing cell_line was created and end with ```.mirun```. To run miRBooking: 
```
java -Xmx10g -jar ~/mirdesign_0.16.jar -s ~/scores -i HV2_LTHY1.mirun --output result -e experiments --experiment-format advanced
```
It generated output folder which contain output files: HV2_D10HY, HV2_D10TC, HV2_R0HY.  


# Generating miRBooking Visualization Table
- This step needs: all average expression files; all miRBooking outputs. All the files was in the same file as the scripts.  
- In the script `generate_miRBooking_visualization_table.py`, edits the input file names for variable f1 and f13, where f1 should be names of the average expression files and f13 should contain the names of the miRBooking output file (the one with all the details). All the  Then run the scripts with:
  ```
  python generate_miRBooking_visualization_table.py
  ```
-  This script would run for some times (this dataset took 10min); hence, while it is running, we can do the next step.
- This script would generate output file: combined_average_expression_updated2. 

# Making miRTables
- This step needs: all miRBooking outputs.
- In the script `make_miRTables.py`, edit the name of input files for vairables mirbook_files. Then run the script: 
  ```
  python make_miRTables.py
  ```
- This should be quick (like under 1min). This step would generate files: 
  ```               
  HV2_D10HY_contribution.tab                        
  HV2_D10HY_miRSum.tab                                     
  HV2_D10TC_contribution.tab 
  HV2_D10TC_miRSum.tab
  HV2_R0HY_contribution.tab
  HV2_R0HY_miRSum.tab
  ```

# Making xgmml_Network_File
- This step will need files:
  - This will need all the previously generated output files. As long as all the outputs files were in the same folder, it should be fine
  - need to create: reduced_glycolysis.network; reduced_glycolysis_network_miRSum.make;  reduced_glycolysis_network_CONTRIBUTION.make;
  - In the file `reduced_glycolysis.network`, states the gene of interests as list of gene names or gene:gene interactions. 
  - In the file `reduced_glycolysis_network_miRSum.make` or `reduced_glycolysis_network_CONTRIBUTION.make`, states the all the file names. And the variable name, type and where to find it. Also the cutoff_value can be changed here too.
- The script '' should be ran like below if the network file stated gene:gene interaction. 
  ```
  python make_miRmRNA_network_isoforms.py reduced_glycolysis_network_miRSum_example.make
  ```
- If the network file stated a list of genes, the script should be ran with a second arguement which could be any random letters input:
  ```
  python make_miRmRNA_network_isoforms.py reduced_glycolysis_network_miRSum_example.make reowinkjdn
  ```
- This generated output file: reduced_glycolysis_miR-mRNA.xgmml. This file can product visualization of network in the software call Cytoscape 

# Visualizing xgmml in Cytoscape
- The xgmml file need to be imported into the software: file -> import -> network -> file. 
