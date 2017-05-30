# Introduction
This documentation was based on the protocal that I ran with Jordan's hypoxia + metabolic stress experimental data. 

# Require file 
It will need average expression files before the calibration function.
The names of average expression files are: moyenne_HV2_D10HY, moyenne_HV2_D10TC, moyenne_HV2_R0HY

# Run miRBooking
Run miRBooking with the data from the database.
A short script contain the name of the testing cell_line was created and end with ```.mirun```. To run miRBooking: 
```
java -Xmx10g -jar ~/mirdesign_0.16.jar -s ~/scores -i HV2_LTHY1.mirun --output result -e experiments --experiment-format advanced
```
It generated output folder which contain output files: HV2_D10HY, HV2_D10TC, HV2_R0HY.  


# Generate miRBooking Visualization Table
- This step needs: all average expression files; all miRBooking outputs. All the files was in the same file as the scripts.  
- In the script `generate_miRBooking_visualization_table.py`, edits the input file name for variable f1 and f13, where f1 should be names of the average expression files and f13 should contain the names of the miRBooking output file (the one with all the details). All the  Then run the scripts with:
````
python generate_miRBooking_visualization_table.py
```
2. covert it to xml file with all the info (alias, symbols, protein name, and label)
-  modify for human database (ex. database link, formatting of names, and other difference between human and mice )
- Blandine should have the modified version
3. modify the make file and network file
4. running python
```
python script_name make_file
```
- is the above script the same script in step 2?
- if doesnt want the pathway arrows to show, put anything behind ... make_file (ex: ```python script_name make_file geingein```);
5. put output file from 4 into cytoscape 




PS: download cytoscape
