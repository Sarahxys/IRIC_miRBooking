# Generating Protein Interaction graph
1. average expression file from Albert or Blandine
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
