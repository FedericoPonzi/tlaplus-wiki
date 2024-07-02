# Pluscal (+cal)

To learn more about pluscal, the language and how the generation process works check this out: [https://github.com/tlaplus/tlaplus/blob/ab14a33e39c78e4c88e81b664b9a8c916b943cab/tlatools/org.lamport.tlatools/src/pcal/PlusCal.tla](https://github.com/tlaplus/tlaplus/blob/ab14a33e39c78e4c88e81b664b9a8c916b943cab/tlatools/org.lamport.tlatools/src/pcal/PlusCal.tla).


## Changing the pluscal translator
The pluscal translator can be thought as the pluscal "compiler" which takes as input pluscal code and produces tla+ in output. 

For this reason, the pluscal translator is versioned. Any change to the pluscal traslator that could result in a different output, will required a verison bump. 

See as an example: https://github.com/tlaplus/tlaplus/pull/978/files

