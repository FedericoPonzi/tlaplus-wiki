
## Exploring the semantic graph
the ExplorerVisitor received an extension to export the semantic graph into dot notation, which can be rendered with GraphViz: <code>java -cp tla2tools.jartla2sany.SANY -d ATLA+Spec.tla dot</code> It optionally includes line numbers if the system property <code>tla2sany.explorer.DotExplorerVisitor.includeLineNumbers=true</code> is set.

