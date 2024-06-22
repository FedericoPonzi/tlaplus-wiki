# TLC Model Checker: How TLC works
This is a concise overview of how TLC model checker works, taken from Markus Kuppe's master thesis: https://lemmster.de/talks/MSc_MarkusAKuppe_1497363471.pdf (section 2.2.3)

---
TLC is an explicit state model checker originally written by Yu et al. [1999] to verify TLA+ specifications. TLC can check a subset of TLA+ that is commonly needed to describe real-world systems, most notably finite systems.

The schematic figure 2.2.3 is a simplified view on TLC. A user of TLC, provides TLC
with a specification and one to $n$ models. The specification - written as a temporal formula (see section 2.1.4) - specifies the system; such as the Bakery mutual exclusion algorithm (Lamport, 1974). Conceptualized, the specification can be seen as the definition of the system's set of behaviors $\Sigma$. A model on the other hand, declares the set of properties $\phi$, that the system is expected to satisfy. In case of the Bakery algorithm, a user may wish to verify the safety property, that no two processes are in the critical section simultaneously. Also, the model declares additional input parameters, such as (system) constants and TLC runtime options. The parameters shall not concern us here.

With regards to checking safety properties, TLC consists of a generator procedure to
create the state graph $G(T)$, which we called the transition system in section 2.1. 

First, the procedure generates the specification's initial states $I$. Afterwards, given a state and the specification's actions, TLC enumerates all possible successor- or next-states (see $\rightarrow$ in section 2.1). Simplified, each successor state s is fed to the second procedure. The second procedure checks, if the state satisfies the model's safety properties $\phi$. If a state is found to violate a property, a counterexample is constructed.
The first procedure generates the state graph on-the-fly by running BFS.4 Consequently, it maintains two sets of states:
* The unseen set S to store newly generated, still unexplored states. A term synonymously used for S in the context of TLC is state queue.
* The seen set C to store explored states. TLC calls C the fingerprint set motivated by the fact that C stores state hashes rather than states (more on that later).

Contrary to regular BFS, TLC maintains a third data structure to construct a counterexample:
* A (state) forest $T$ of rooted in-trees to construct the shortest path from any state $s_n \in S$ back to an initial state $s_o \in I$. The states in $I$ correspond to the roots in $T$.
The PlusCal pseudo-code in algorithm 1 shows a simplified variant of TLC's algorithm to check safety properties (full listing in section B.1). Readers, unfamiliar with TLA+ or PlusCal, are referred to the reference list in appendix A on page 117 for a brief introduction of the language constructs. First, TLC generates the initial states atomically and adds them to the set of unseen states $S$. The init loop checks each initial state for a violation. 

If the state does not violate the properties, it is added to C (algorithm 1 line 6 to line 13). The second $scsr$ loop executes BFS (algorithm 1 line 14 to line 30). The $SuccessorsOf$ operation corresponds to the next state relation (see → in section 2.1). The $succ \in ViolationStates$ check represents the procedure to verify the safety properties $\phi S$ .
TLC naïvely parallelizes BFS by concurrently executing the while loop scsr with multiple threads - called workers.

The global data structures $S$, $C$, $T$ are shared by all
workers. Consequently, each data structure has to be guarded by locks to guarantee consistency. Sharing $S$ among all workers achieves optimal load-balancing. We call this mode of execution parallel TLC. TLC can also execute on a network of computers
in what is called distributed mode [Kuppe, 2012]. Distributed TLC speeds up model checking.

TLC is implemented in and available on all operating systems supported by Java.

TLC runs on commodity hardware found in desktop computers, up to high-end servers equipped with hundreds of cores and terabytes of memory. It is capable of checking large-scale models, i.e. the maximum checkable size of a model is not bound to the available memory. Instead, TLC keeps the in-memory working set small:

1. TLC swaps unexplored states in the unseen set S from memory to disk. The unseen set S has a fixed space requirement.
2. TLC constantly puushes the forest to disk. Iff a violation is found, TLC reads the forest back to disk to construct the counterexample.
3. The seen set $C$ - whenever it exceeds its allocated memory - extends to disk.

1 and 2 enable TLC to allocate the majority of memory to the seen set $C$. When the seen set $C$ exceeds its allocated memory (3), TLC is said to run disk-based model checking. Once TLC switches to disk-based model checking, its performance, i.e. the number of states generated per unit of time, drops significantly. Still, disk-based model checking has its place. It makes the verification of state space tractable, for which the primary memory alone is insufficient, e.g. Newcombe [2014] gives an account where TLC ran for several weeks to check a state space of approximately $2^{34}$ states.

TLC supports the use of TLC module overwrites in TLA+ and PlusCal specifications. A TLC module overwrite is a static Java method which matches the signature of a TLA+ operator. With a TLC module overwrite in place, TLC delegates the evaluation of the corresponding TLA+ operator to the Java method. Generally, this is assumed to be faster. A TLC module overwrite is stateless and cannot access its context. 

Additionally, TLC module overwrites make the Java library available to TLA+ users.

\[
\begin{array}{l}
        \textbf{algorithm} \; \text{ModelChecker} \{ \\
        \quad \textbf{variables} \\
        \quad S := \text{SetOfAllPermutationsOfInitials}(StateGraph); \\
        \quad C := \{\}, \; state := null, \; successors := \{\}; \\
        \quad i := 1, \; counterexample := \langle \rangle, \; T := \langle \rangle; \\
        \\
        \quad \textbf{init: while} \; (i \leq \text{Len}(S)) \{ \\
        \quad \quad state := \text{Head}(S); \\
        \quad \quad C := C \cup \{state\}; \\
        \quad \quad i := i + 1; \\
        \quad \quad \textbf{if} \; (state \in \text{ViolationStates}) \{ \\
        \quad \quad \quad counterexample := \langle state \rangle; \; \textbf{goto} \; trc; \\
        \quad \quad \} \textbf{end if}; \\
        \quad \} \textbf{end while}; \\
        \\
        \quad \textbf{scsr: while} \; (\text{Len}(S) \neq 0) \{ \\
        \quad \quad state := \text{Head}(S); \; S := \text{Tail}(S); \\
        \quad \quad successors := \text{SuccessorsOf}(state, \; StateGraph, \; C); \\
        \quad \quad \textbf{if} \; (successors = \{state\}) \{ \\
        \quad \quad \quad counterexample := \langle null \rangle; \; \textbf{goto} \; trc; \\
        \quad \quad \} \textbf{end if}; \\
        \quad \quad \textbf{each: while} \; (successors \neq \{\}) \{ \\
        \quad \quad \quad \textbf{with} \; (succ \in successors) \{ \\
        \quad \quad \quad \quad successors := successors \setminus \{succ\}; \\
        \quad \quad \quad \quad C := C \cup \{succ\}; \; S := S \circ \{succ\}; \\
        \quad \quad \quad \quad T := T \circ \langle (state, succ) \rangle; \\
        \quad \quad \quad \quad \textbf{if} \; (succ \in \text{ViolationStates}) \{ \\
        \quad \quad \quad \quad \quad counterexample := \langle succ \rangle; \; \textbf{goto} \; trc; \\
        \quad \quad \quad \quad \} \textbf{end if}; \\
        \quad \quad \quad \} \textbf{end while}; \\
        \quad \quad \} \textbf{end while}; \\
        \\
        \quad \textbf{goto} \; Done; \\
        \\
        \quad \textbf{trc: while} \; (\text{TRUE}) \{ \\
        \quad \quad \textbf{if} \; (\text{Head}(counterexample) \notin \text{StateGraph.initials}) \{ \\
        \quad \quad \quad counterexample := \langle \text{Predecessor}(T, \text{Head}(counterexample)) \circ counterexample \rangle; \\
        \quad \quad \} \textbf{else} \{ \\
        \quad \quad \quad \textbf{goto} \; Done; \\
        \quad \quad \} \textbf{end if}; \\
        \quad \} \textbf{end while}; \\
        \\
        \quad \textbf{Done:}; \\
        \}
        \end{array}
\]

---

## What's the difference between the behavior graph and state exploration graph?

The behavior graph is the cross product of the state graph and the tableau that represents the liveness property.

