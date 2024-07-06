# Debugging and rootcausing issues

## Start from the MRE

Minimum Reproducible Example. Often, if you can get one half of your problem is solved. This is why is so important to provide one when creating issues. The smaller the state graph needed to reproduce the issue, the better.


## Write a regression test

Write a test, and make sure it fails. After your fix is applied, the test should start passing again. When writing the regression test, make sure to think what could happen if you have multiple workers involved. Do you need to test that as well?


## Generate the state graph

See [generating state graphs](../using/generating-state-graphs.md) page. State graphs can help visualize what's going on.


## Run your spec with assertion turned on.

TLC has a `-ea` option that will enable some assertions in the code. It might be able to help catching some edge case hit by your code.

## Run with debugging logs

I don't find these particular helpful, but it's worth trying. Run tlc with `-debug` on and you will get additional debug logs in output.

## Attaching a debugger
To the final jar:
```
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=*:5005 -jar ./dist/tla2tools.jar /tmp/genereto/Github710a.tla 
```
## Eliminate randomness
To enanche reproducibility. Use -fp and -seed.
```
java -jar ./dist/tla2tools.jar -lncheck final -fpmem 0.65 -fp 120 -seed -8286054000752913025  -workers 10 /tmp/genereto/Github710a.tla 
```


# CustomBuild parameters
customBuild.xml supports different parameters to tune the tests. These can also be combined.

## Running a test with a debugger
`-Dtest.suspend=y` will suspend the execution and give you time to hook up a debugger before running a test.

```
ant -f customBuild.xml compile-test test-set -Dtest.suspend=y -Dtest.testcases="tlc2/tool/InliningTest*"
```

## Specify number of threads
By default uses all your cores. You can specify a custom amount using `-Dthreads=4`:

```
ant -f customBuild.xml test -Dthreads=4
```

## Halt tests when a failure is found
By default the test target will run all the test and report success or failure only at the end. It can be convinient to just stop execution as soon as an error is found, with`-Dtest.halt=true`.
```
ant -f customBuild.xml test -Dtest.halt=true
```












