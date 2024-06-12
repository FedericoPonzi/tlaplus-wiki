# Codebase Idiosyncrasies

As a 20 year old code base, one can expect idiosyncrasies that do not match current best practice. There are also inherent idiosyncrasies to any language interpreter. Maintaining functionality and performance is the most important concern. However whenever these idiosyncrasies can be removed without compromising those, they should be.

### Dynamic Class Loading
This project makes extensive use of Classloaders. This can make it slightly more difficult to debug / statically analyze. Critical usecases are called out here.

#### Operators / Modules
The ClassLoader is used to load both standard modules and user created modules. The standard modules can be found here:
- [src/tlc2/module](../src/tlc2/module)
- [src/tla2sany/StandardModules](../src/tla2sany/StandardModules)

The [SimpleFilenameToStream](../src/util/SimpleFilenameToStream.java) class is used to read in this information, and contains the logic about standard module directories. It is where the ClassLoader is used. [TLAClass](../src/tlc2/tool/impl/TLAClass.java) is also used for a similar purpose, used to loader certain built in classes.

The classpath of the created jar explicitly includes the CommunityModules such that they can be loaded if available.
```
CommunityModules-deps.jar CommunityModules.jar
```

Users can also create custom operators and modules and load them similarly.

The methods are invoked with:
``` java
mh.invoke
mh.invokeExplict
```

And this is done in a small number of locations:
[MethodValue](../src/tlc2/value/impl/MethodValue.java)
[CallableValue](../src/tlc2/value/impl/CallableValue.java)
[PriorityEvaluatingValue](../src/tlc2/value/impl/PriorityEvaluatingValue.java)


#### FPSet Selection

FPSets are dynamically selected using a system property and loaded with a ClassLoader in the [FPSetFactory](../src/tlc2/tool/fp/FPSetFactory.java).

#### Misc
- [TLCWorker](../src/tlc2/tool/distributed/TLCWorker.java): Used to load certain sun class dependencies if available.
- [BlockSelectorFactory](../src/tlc2/tool/distributed/selector/BlockSelectorFactory.java): Used to modularity load a custom BlockSelectorFactory.
- [TLCRuntime](../src/util/TLCRuntime.java): Used to get processId

### Notable Mutable Static State
The original codebase was written with the intention of being run from the command line only.

There is a significant amount of static state. While much has been removed
- [util/UniqueString.java](../src/util/UniqueString.java):
- [util/ToolIO.java](../src/util/ToolIO.java): Used for 
- [tlc2/TLCGlobals.java](../src/tlc2/TLCGlobals.java):

### Testing Brittleness

The end to end test suite is a very powerful tool of the project. It does have a reliance on the execution occurring in a very precise order. There are certain race conditions in the codebase that can cause some inconsistency in execution statistics, even while providing correct behavior. This can cause some tests to fail. Additionally, there are some race condition bugs. Additonally. It is not always easy to determine which case it falls into, and so categorizing / fixing these test cases should lead either codebase or test suite improvements. 

#### Independently Run Tests

In order to allow tests to be independently run, we add one of the following tags depending on whether it is a standard test or a TTraceTest

``` java
@Category(IndependentlyRunTest.class)
@Category(IndependentlyRunTTraceTest.class)
```

In general, these should be used sparingly, and instead if a test fails when run with others, the root cause should be discovered and fixed.

#### Unique String ordering reliance

As mentioned above, unique strings replace strings with a consistent integer token for faster comparison. That token is monotonically increasing from when the unique string is generated. When comparing unique strings, it compares the tokens, meaning the ordering of the UniqueString based collection is dependant on the ordering of the creation of strings. This can break tests that hardcode the ordering of the result output when they are not run in isolation. This isn't necessarily a fundamental problem with the system, as the output is consistent based on TLA+ semantics which does not differ based on order. 

The tests that fail for this reason are marked as independent tests, but grouped under 

``` xml
<id>unique-string-conflicts</id>
```

in [pom.xml](../pom.xml). Their reason for failure is known.

#### Debugger Tests
The AttachedDebugger currently only terminates on process exit. For that reason, all debugger tests are marked with the annotation below, and run as independent processes.

``` java
@Category(DebuggerTest.class)
```

### Primitive Versions of Standard Data Structures

The standard collections in the Java standard library store only objects. Some custom collections are required that can store and/or be indexed by primitives. These are needed for performance reasons.
- [LongVec](../src/tlc2/util/LongVec.java)
- [IntStack](../src/tlc2/util/IntStack.java)
- [SetOfLong](../src/tlc2/util/SetOfLong.java)
- [ObjLongTable](../src/tlc2/util/ObjLongTable.java)
- [LongObjTable](../src/tlc2/util/LongObjTable.java)

> Note: There may be more not listed here, but ideally they should be added.

### Unchecked Casting
As a language interpreter, there are a number of Abstract Syntax Tree node types. In many cases, functions use unchecked casts to resolve these node types, often after using if statements to check the nodes type.

To find all the classes / functions that do this, search for:
```
@SuppressWarnings("unchecked")
```

Whenever possible unchecked casts should be replaced with [pattern matching instanceof](https://docs.oracle.com/en/java/javase/17/language/pattern-matching-instanceof-operator.html). This generally is a good fit for most of the code in the codebase.

### Dead Code
This project was initiated prior to "good" version control. Therefore modern anti-patterns such as commenting out code and leaving unused methods, classes, etc have propagated. Significant amounts of dead code have been removed. Because of the use of reflection / classloaders, static analysis tools may indicate certain items are unused when they are actually depended on. Dead code removal needs to be done in conjunction with testing and exploration.

#### Acceptable Dead Code
A certain amount of dead code may have explanatory purpose.
- Standard methods implemented on data structures: ideally they should be tested, but some are not.
- Semantic variables / properties that are set appropriately but unread: They serve as a form of comment. Misleading examples of these should be removed.
- Small amounts of inline, commented out code that changes particular functionality or configures the codebase.
- Tested classes that implement used interfaces, where the class is not currently used: These still have explanatory purpose. 

Any of this code should be removed when it loses relevance or accuracy.

#### Dead Code to be Removed
- Commented out methods
- Orphan classes
- Large amounts of inline commented out code without sufficient explanatory power
- Unused, untested, non-standard methods: Version control can be used if they are needed, but they add confusion and may be broken


### Inconsistent Formatting
The formatting of certain classes is inconsistent and doesn't work well with modern IDEs. Ideally an autoformatter would be run on the codebase to fix this. However, as this is a fork of the primary codebase, it is left unrun to allow better diffs with the upstream repo.       

### JMX / Eclipse Integrations
This project was initially developed as an Eclipse project, so it includes a number of Eclipse libraries and technologies. Most of them integrate rather seamlessly as dependencies, while enabling specialized diagnostic functionality for Eclipse.

The project has [AspectJ](https://en.wikipedia.org/wiki/AspectJ) source that can be compiled in for additional diagnostics. It is also required for Long Tests to pass. The sources are located in [src-aspectj](../src-aspectj).

Additionally this project uses Java Management Extensions for diagnostic purposes. They are always used an can be a source of memory leaks. 


### Java Pathfinder
There are java pathfinder tests in the project. Sources are located in [test-verify](../test-verify/). Additional information on these tests can be found in [test-verify/README](../test-verify/README.md).