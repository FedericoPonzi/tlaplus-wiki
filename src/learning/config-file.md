# Config files
The config files are used for TLC model checking.

The possible contents of the config file itself are presented below. The config file has `.cfg` extension, and usually has the same name of your spec (`.tla` file).

TODO: values supported in config files. Typed model values.

### Table Of Contents:
<!-- toc -->

## Supported Sections
### `Constants` or `Constant` (they're aliases)
You can use it to specify the constant used in the model.
```
CONSTANTS
    Processes = {1,2,3}
```
Equivalent to:
```
CONSTANT
    Processes = {1,2,3}
```
### `SPECIFICATION`
The name of the predicate that usually has the form of: `Init /\ [][Next]_vars /\backslash`.

Usually per convention it's called `Spec`.
```
SPECIFICATION
    Spec
```

### `INVARIANT` or `INVARIANTS` (they're aliases)
Invariants that you want to verify.

```
INVARIANT
    TypeOk \* Always verify your types!
```
### `PROPERTIES` or `PROPERTY` (they're aliases)
Temporal properties you want to verify.

```
PROPERTIES
    Termination
```

### `CONSTRAINT` or `CONSTRAINTS` (they're aliases)
Used to restrict the state space to be explored. Helps restricting unbounded models.


### `SYMMETRY`
Helps reducing the state space to explore by removing symmetric states. You can't check liveness properties when symmetry is used.
See: https://federicoponzi.github.io/tlaplus-wiki/codebase/wishlist.html#liveness-checking-under-symmetry-difficulty-high-skills-java-tla

### `VIEW`

### `CHECK_DEADLOCK`

### `POSTCONDITION`

### `ALIAS`

### `INIT`

### `NEXT`


## A copy-pastable example:
```
\* Add statements after this line.
CONSTANTS

SPECIFICATION
    Spec

INVARIANT
    TypeOK

PROPERTIES
\*    Termination

\* Check presence of deadlocks. It's true by default.
CHECK_DEADLOCK 
    FALSE
```

## Resources
Check the EBNF and more info on the apalache documentation [here](https://apalache.informal.systems/docs/apalache/tlc-config.html).
