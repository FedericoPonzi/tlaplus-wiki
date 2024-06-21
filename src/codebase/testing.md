# Testing

## test-dist:
Executes accompanying unit tests on jar file
```
ant -f customBuild.xml clean compile compile-test test-dist
```

To speedup, you can modify customBuild.xml and modify junit line inside of test-dist:
```
<junit printsummary="yes" haltonfailure="${test.halt}" haltonerror="${test.halt}" forkmode="perTest" fork="yes">
```

to use more threads: `threads="8"`
```
<junit printsummary="yes" haltonfailure="${test.halt}" haltonerror="${test.halt}" forkmode="perTest" fork="yes" threads="8">
```

