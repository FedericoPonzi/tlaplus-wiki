# Generating Animations of State Changes

Writing animation specifications is straightforward. There is a module that provides TLA+ definitions of SVG primitives. Below are a few examples. The first three are for TLC, and the other ones are for Will Schultz's TLA-web. Despite being for different systems, the definitions are quite similar.

* https://github.com/tlaplus/Examples/blob/master/specifications/ewd998/EWD998_anim.tla
* https://github.com/tlaplus/Examples/blob/master/specifications/ewd687a/EWD687a_anim.tla
* https://github.com/tlaplus/Examples/blob/master/specifications/ewd840/EWD840_anim.tla
* https://github.com/will62794/tla-web/blob/master/specs/EWD998.tla#L123-L201
* https://github.com/will62794/tla-web/blob/master/specs/AbstractRaft_anim.tla#L210-L280

The animations can be created in the Toolbox with:

1. Check model [BlockingQueue.cfg](BlockingQueue.cfg)
2. Set ```Animation``` as the trace expression in the Error-Trace console
3. Hit Explore and export/copy the resulting trace to clipboard
4. Paste into http://localhost:10996/files/animator.html

Without the Toolbox, something similar to this:

1. Check model [BlockingQueue.cfg](BlockingQueue.cfg) with ```java -jar tla2tools.jar -deadlock -generateSpecTE BlockingQueue``` ('-generateSpecTE' causes TLC to generate [SpecTE.tla](SpecTE.tla)/.cfg)
2. State trace expression ```Animation``` ([BlockingQueueAnim.tla](BlockingQueueAnim.tla))in SpecTE.tla
3. Download https://github.com/tlaplus/CommunityModules/releases/download/20200107.1/CommunityModules-202001070430.jar
4. Check SpecTE with ```java -jar tla2tools.jar:CommunityModules-202001070430.jar tlc2.TLC SpecTE```
5. Copy trace into http://localhost:10996/files/animator.html (minor format changes needed)

