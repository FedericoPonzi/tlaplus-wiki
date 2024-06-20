# Generating stage graphs
first generate dot file:
```
java  -cp ./dist/tla2tools.jar tlc2.TLC -dump dot test.dot /home/fponzi/dev/tla+/Examples/specifications/glowingRaccoon/clean.tla
```
then generate graph: 
 * [http://www.webgraphviz.com/](http://www.webgraphviz.com/)
 * From CLI: `dot -Tps filename.dot -o outfile.ps`

If you use intellij, there is a plugin to visualize/edit .dot files in the ide.

