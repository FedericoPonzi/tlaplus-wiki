## Coverage (draft)
The number of times each action was taken to construct a new state. Using this information, we can identify actions that are
never taken and which might indicate an error in the specification.

## How to read coverage?

```
// Determine if the mapping from the action's name/identifier/declaration to the
// action's definition is 1:1 or 1:N.
//
// Act == /\ x  = 23
//        /\ x' = 42
// vs
// Act == \/ /\ x  = 23
//           /\ x' = 42
//        \/ /\ x  = 123
//           /\ x' = 4711
// or
// Act == (x  = 23 /\ x' = 42) \/ (x  = 123 /\ x' = 4711)
//
// For a 1:1 mapping this prints just the location of Act. For a 1:N mapping it
// prints the location of Act _and_ the location (in shortened form) of the actual
// disjunct.
			// 1:1
			return String.format("<%s %s>", this.action.getName(), declaration);
		} else {
			// 1:N
			return String.format("<%s %s (%s %s %s %s)>",
```
At high level, it's defined as:
```
MP.printMessage(EC.TLC_COVERAGE_VALUE_COST,
        new String[] { indentBegin(level, TLCGlobals.coverageIndent, getLocation().toString()),
                String.valueOf(count), String.valueOf(cost) });
```

as an example:
```
<Next line 13, col 1 to line 13, col 4 of module Github845 (15 8 17 24)>: 0:2
```
0:2 at the end mean:
* number of distinct states discovered after taking this action
* number of times this action was chosen.

if it only has a value, like this:

```
line 23, col 6 to line 23, col 40 of module M: 1536
```
that's the count.


for lines like these:
```
    [junit]   line 23, col 6 to line 23, col 40 of module M: 1536
    [junit]   |line 23, col 18 to line 23, col 40 of module M: 192:4608
    [junit]   ||line 23, col 19 to line 23, col 28 of module M: 192
    [junit]   ||line 23, col 33 to line 23, col 39 of module M: 192

```
the pipe prefix is for subcomponent of the action. In this case, line 23 is:
```
  /\ active' \in [{23,42,56} -> BOOLEAN]
```


## What is cost?
Cost is the number of operations performed to enumerate the elements of a set or sets, should
enumeration be required to evaluate an expression

At top level action it usually points to the max of the cost of all children nodes I think?

Profiling a specification is similar to profiling implementation code: During model checking, the profiler collects evaluation metrics about the invocation of expressions, their costs, as well as action metrics. The number of invocations equals the number of times an expression has been evaluated by the model checker. Assuming an identical, fixed cost for all expressions allows to identify the biggest contributor to overall model checking time by looking at the number of invocations. This assumption however does not hold for expressions that require the model checker to explicitly enumerate data structures as part of their evaluation. For example, let S be a set of natural numbers from N to M such that N << M and \A s \in SUBSET S : s \subseteq S be a expression. This expression will clearly be a major contributor to model checking time even if its number of invocations is low. More concretely, its cost equals the number of operations required by the model checker to enumerate the powerset of S. Users can override such operators with more efficient variants. Specifically, TLC allows operators to be overridden with Java code which can often be evaluated orders of magnitudes faster. 





## Good to know:
### Why isn't Next's coverage reported?
TLC does not evaluate Next but [turns](https://github.com/tlaplus/tlaplus/blob/master/tlatools/org.lamport.tlatools/src/tlc2/tool/impl/Tool.java#L261-L413) its two disjuncts into separate sub-actions internally.

### Why isn't ASSUME's coverage reported?
* Coverage statistics are not collected for ASSUME statements.

* Coverage statistics are not reported for operators called (directly or indirectly) from top-level actions (with this patch I see that such expressions are only reported if they have 0 cost).


## More references
* https://tla.msr-inria.inria.fr/tlatoolbox/doc/model/profiling.html
