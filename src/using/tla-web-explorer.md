# TLA+ Web Explorer
[Homepage](https://will62794.github.io/tla-web/#!/home?specpath=.%2Fspecs%2FTwoPhase.tla) and the source code is hosted on [GitHub](https://github.com/will62794/tla-web).

The current version of the tool utilizes the TLA+ tree-sitter grammar for parsing TLA+ specs and implements a TLA+ interpreter/executor on top of this in Javascript. This allows the tool to interpret specs natively in the browser, without relying on an external language server. The Javascript interpreter is likely much slower than TLC, but efficient model checking isn't currently a goal of the tool.

Watch the presentation:

<iframe width="1333" height="485" src="https://www.youtube.com/embed/kSSWmxQLvmw" title="TLA+ Conf - William Schultz - Towards Interactive Formal Specs" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

