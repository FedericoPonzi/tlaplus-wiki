# TLC Model Checker
TLC is an explicit state model checker originally written by Yu et al. [1999] to verify TLA+ specifications. TLC can check a subset of TLA+ that is commonly needed to describe real-world systems, most notably finite systems.

To run it, you will need Java runtime environment version 11.

### Table Of Contents:
<!-- toc -->

## Getting the latest stable release
You can always download the latest stable release from the release page on github: https://github.com/tlaplus/tlaplus/releases/


## Building TLC from sources
You will need JDK 11 and ant.
1. Download the repository from github: https://github.com/tlaplus/tlaplus
2. Go to tlaplus/tlatools/org.lamport.tlatools and run: `ant -f customBuild.xml compile dist`

You can then find the jar file inside tlaplus/tlatools/org.lamport.tlatools/dist/tla2tools.jar

## Running the tla2tools.jar
To run the TLC model checker:
```
java -jar ./dist/tla2tools.jar <MySpec.tla>
```
the cli will automatically use the config file called MySpec.cfg in that same directory, if present.

### JVM options
See https://docs.oracle.com/en/java/javase/20/gctuning/available-collectors.html#GUID-F215A508-9E58-40B4-90A5-74E29BF3BD3C

> If (a) peak application performance is the first priority and (b) there are no pause-time requirements or pauses of one second or longer are acceptable,
> then let the VM select the collector or select the parallel collector with -XX:+UseParallelGC.

Simulation, nor Model Checking seem to work well with G1GC, but Trace Validation does not for some reason, and is much slower unless -XX:+UseParallelGC is used.

In all cases, pauses don't matter, only throughput does.
```
java -XX:+UseParallelGC
```


## CLI Parameters
You can always access the latest information by using
```
java -jar ./dist/tla2tools.jar -help
```
Not all options are exposed through it though. These are the options that are supported but missing from the -help:

* `-lncheck`: Check liveness properties at different times of model checking. Supported values are "default", "final", "seqfinal". "default" will check liveness at every step, while final will search it only at the end of the exploration. "seqfinal" is anlias for "final". -lncheck final option postpones the search of Strongly Connected Components in the behavior graph until after the safety check has been completed

These are all the options outputted from the cli at the time of writing from master:
```
    -aril num
            adjust the seed for random simulation; defaults to 0
    -checkpoint minutes
            interval between check point; defaults to 30
    -cleanup
            clean up the states directory
    -config file
            provide the configuration file; defaults to SPEC.cfg
    -continue
            continue running even when an invariant is violated; default
            behavior is to halt on first violation
    -coverage minutes
            interval between the collection of coverage information;
            if not specified, no coverage will be collected
    -deadlock
            if specified DO NOT CHECK FOR DEADLOCK. Setting the flag is
            the same as setting CHECK_DEADLOCK to FALSE in config
            file. When -deadlock is specified, config entry is
            ignored; default behavior is to check for deadlocks
    -debug
            print various debugging information - not for production use
            
    -debugger nosuspend
            run simulation or model-checking in debug mode such that TLC's
            state-space exploration can be temporarily halted and variables
            be inspected. The only debug front-end so far is the TLA+
            VSCode extension, which has to be downloaded and configured
            separately, though other front-ends could be implemeted via the
            debug-adapter-protocol.
            Specifying the optional parameter 'nosuspend' causes
            TLC to start state-space exploration without waiting for a
            debugger front-end to connect. Without 'nosuspend', TLC
            suspends state-space exploration before the first ASSUME is
            evaluated (but after constants are processed). With 'nohalt',
            TLC does not halt state-space exploration when an evaluation
            or runtime error is caught. Without 'nohalt', evaluation or
            runtime errors can be inspected in the debugger before TLC
            terminates. The optional parameter 'port=1274' makes the
            debugger listen on port 1274 instead of on the standard
            port 4712, and 'port=0' lets the debugger choose a port.
            Multiple optional parameters must be comma-separated.
            Specifying '-debugger' implies '-workers 1'.
    -depth num
            specifies the depth of random simulation; defaults to 100
    -dfid num
            run the model check in depth-first iterative deepening
            starting with an initial depth of 'num'
    -difftrace
            show only the differences between successive states when
            printing trace information; defaults to printing
            full state descriptions
    -dump file
            dump all states into the specified file; this parameter takes
            optional parameters for dot graph generation. Specifying
            'dot' allows further options, comma delimited, of zero
            or more of 'actionlabels', 'colorize', 'snapshot' to be
            specified before the '.dot'-suffixed filename
    -dumpTrace format file
            in case of a property violation, formats the TLA+ error trace
            as the given format and dumps the output to the specified
            file.  The file is relative to the same directory as the
            main spec. At the time of writing, TLC supports the "tla"
            and the "json" formats.  To dump to multiple formats, the
            -dumpTrace parameter may appear multiple times.
            The git commits 1eb815620 and 386eaa19f show that adding new
            formats is easy.
            
    -fp N
            use the Nth irreducible polynomial from the list stored
            in the class FP64
    -fpbits num
            the number of MSB used by MultiFPSet to create nested
            FPSets; defaults to 1
    -fpmem num
            a value in (0.0,1.0) representing the ratio of total
            physical memory to devote to storing the fingerprints
            of found states; defaults to 0.25
    -gzip
            control if gzip is applied to value input/output streams;
            defaults to 'off'
    -h
            display these help instructions
    -maxSetSize num
            the size of the largest set which TLC will enumerate; defaults
            to 1000000 (10^6)
    -metadir path
            specify the directory in which to store metadata; defaults to
            SPEC-directory/states if not specified
    -noGenerateSpecTE
            Whether to skip generating a trace exploration (TE) spec in
            the event of TLC finding a state or behavior that does
            not satisfy the invariants; TLC's default behavior is to
            generate this spec.
    -nowarning
            disable all warnings; defaults to reporting warnings
    -postCondition mod!oper
            evaluate the given (constant-level) operator oper in the TLA+
            module mod at the end of model-checking.
    -recover id
            recover from the checkpoint with the specified id
    -seed num
            provide the seed for random simulation; defaults to a
            random long pulled from a pseudo-RNG
    -simulate
            run in simulation mode; optional parameters may be specified
            comma delimited: 'num=X' where X is the maximum number of
            total traces to generate and/or 'file=Y' where Y is the
            absolute-pathed prefix for trace file modules to be written
            by the simulation workers; for example Y='/a/b/c/tr' would
            produce, e.g, '/a/b/c/tr_1_15'
    -teSpecOutDir some-dir-name
            Directory to which to output the TE spec if TLC generates
            an error trace. Can be a relative (to root spec dir)
            or absolute path. By default the TE spec is output
            to the same directory as the main spec.
    -terse
            do not expand values in Print statements; defaults to
            expanding values
    -tool
            run in 'tool' mode, surrounding output with message codes;
            if '-generateSpecTE' is specified, this is enabled
            automatically
    -userFile file
            an absolute path to a file in which to log user output (for
            example, that which is produced by Print)
    -view
            apply VIEW (if provided) when printing out states
    -workers num
            the number of TLC worker threads; defaults to 1. Use 'auto'
            to automatically select the number of threads based on the
            number of available cores.
```

## Undocumented flags
Some flags can be set passing java parameters to tlc, they are mostly undocumented (for now) and can be found by searching for "Boolean.getBoolean" in the codebase.


## Good to know:
When using the  '-generateSpecTE' you can version the generated specification by doing:

    ./tla2tools.jar -generateSpecTE MySpec.tla && NAME="SpecTE-$(date +%s)" && sed -e "s/MODULE SpecTE/MODULE $NAME/g" SpecTE.tla > $NAME.tla

If, while checking a SpecTE created via '-generateSpecTE', you get an error message concerning
CONSTANT declaration and you've previous used 'integers' as model values, rename your
model values to start with a non-numeral and rerun the model check to generate a new SpecTE.

If, while checking a SpecTE created via '-generateSpecTE', you get a warning concerning
duplicate operator definitions, this is likely due to the 'monolith' specification
creation. Try re-running TLC adding the 'nomonolith' option to the '-generateSpecTE'
parameter.

When using more than one worker, the reported depth might differ across runs. For small models, use a single worker. For large models, the diameter will almost always appear deterministic.