# Introduction
This page will document the workflow that I used to identify leader and followers. As for now, it is just some thoughts. It will probably change depends on how Zohra's scripts work.

# Runing miRBooking
For identifying followers of a gene: 
- The script that I used to run miRBooking will modify the expression level of the target/gene of interests (typically 100-6000 with increment of 100). 
- A output file 'result' will be rotated to make it readable. This will generate file 'resultrt'. 
- A perl script 'ChangeInExpression' would be ran to identify 
