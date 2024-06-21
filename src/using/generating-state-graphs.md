# Generating stage graphs
TLC support the generation of a dot file that can be used to visualize the state space.

first generate dot file:
```
java  -cp ./dist/tla2tools.jar tlc2.TLC -dump dot test.dot /home/fponzi/dev/tla+/Examples/specifications/glowingRaccoon/clean.tla
```
then visualize the graph: 
 * [http://www.webgraphviz.com/](http://www.webgraphviz.com/)
 * From CLI: `dot -Tps filename.dot -o outfile.ps`

If the state graph is large, you can try to visualize it using:

* https://cytoscape.org/index.html
* https://gephi.org/

If you use intellij, there is a plugin to visualize/edit .dot files in the ide.

From the TLA+ Toolbox:

if dot is not installed in the standard path, please adjust the dot path in the Toolbox's preferences (File > Preferences > TLA+ Preferences > PDF Viewer > Specify dot command). Afterwards check "Visualize state graph after completion of model checking" before you run model-checking on the "Advanced Options" page of the model editor (see attached screenshot). The Toolbox will then add a tab to the model editor showing the state graph once model checking has completed.

Note that the visualization is only useful for small state spaces. For larger ones, the dot rendering will timeout. In this case you can try to render the dot output file manually on the command line with a different dot settings, i.e. layout. The dot file is found in the model subdirectory of your specification. 


