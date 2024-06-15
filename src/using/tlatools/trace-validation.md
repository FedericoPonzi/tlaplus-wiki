# Trace Validation
This is a comment from Markus Kuppe from [Github](https://github.com/etcd-io/raft/issues/111#issuecomment-1829035938).

Trace Validation (TV) essentially adopts this approach by mapping recorded implementation states to TLA+ specification states. This method is practical as it derives implementation states directly from the execution of the implementation, avoiding the complexities of symbolic interpretation of the source code. However, TV is not exhaustive. It verifies that only a certain subset of all potential system executions aligns with the behaviors defined in the specification. Nevertheless, this is not necessarily a limitation if the chosen subset of system executions is sufficiently diverse. For example, in CCF, among other discrepancies, we were able to find two significant data-loss issues using TV with just about a dozen system executions.

    It also allows you to use the specification to drive the testing of the implementation.

In CCF, this feature is still on our roadmap. However, manually translating TLA+ counterexamples (violations of the spec's correctness properties) into inputs for the implementation was straightforward.

## Why Trace Validation is Useful
You can use it to check that the implementation is in sync with your spec, or even go as far as generating a complete spec from the code. See discussion [here](https://github.com/eatonphil/raft-rs/issues/1#issuecomment-1854576123).

# Depth-First Search
Depth-First Search for trace validation.

To run a DFS, use:
```
JVM_OPTIONS=-Dtlc2.tool.queue.IStateQueue=StateDeque
```

StateDequeue was added in Jan 2024.

## Resources:
* [Verifying Software Traces Against a Formal Specification with TLA+ and TLC](https://pron.github.io/files/Trace.pdf)