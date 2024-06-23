# Summary
You can access to a quick pdf summary about the tla+ language here: [https://lamport.azurewebsites.net/tla/summary-standalone.pdf](https://lamport.azurewebsites.net/tla/summary-standalone.pdf).

This is a web accessible version.

----


# Summary of TLA ${ }^{+}$ 
* Module-Level Constructs 
* The Constant Operators 
* Miscellaneous Constructs 
* Action Operators 
* Temporal Operators 
* User-Definable Operator Symbols 
* Precedence Ranges of Operators 
* Operators Defined in Standard Modules. 
* ASCII Representation of Typeset Symbols 1

## Module-Level Constructs

## $\Gamma$

 MODULE $M$Begins the module or submodule named $M$.

EXTENDS $M_{1}, \ldots, M_{n}$

Incorporates the declarations, definitions, assumptions, and theorems from the modules named $M_{1}, \ldots, M_{n}$ into the current module.

CONSTANTS $C_{1}, \ldots, C_{n}^{(1)}$

Declares the $C_{j}$ to be constant parameters (rigid variables). Each $C_{j}$ is either an identifier or has the form $C\left(\_, \ldots, \_\right)$, the latter form indicating that $C$ is an operator with the indicated number of arguments.

$$
\text { VARIABLES } x_{1}, \ldots, x_{n}^{(1)}
$$

Declares the $x_{j}$ to be variables (parameters that are flexible variables).

## ASSUME $P$

Asserts $P$ as an assumption.

$F\left(x_{1}, \ldots, x_{n}\right) \triangleq \exp$

Defines $F$ to be the operator such that $F\left(e_{1}, \ldots, e_{n}\right)$ equals exp with each identifier $x_{k}$ replaced by $e_{k}$. (For $n=0$, it is written $F \triangleq \exp$.)

$f[x \in S] \triangleq \exp ^{(2)}$

Defines $f$ to be the function with domain $S$ such that $f[x]=\exp$ for all $x$ in $S$. (The symbol $f$ may occur in exp, allowing a recursive definition.)

(1) The terminal S in the keyword is optional.

(2) $x \in S$ may be replaced by a comma-separated list of items $v \in S$, where $v$ is either a comma-separated list or a tuple of identifiers.

INSTANCE $M$ WITH $p_{1} \leftarrow e_{1}, \ldots, p_{m} \leftarrow e_{m}$

For each defined operator $F$ of module $M$, this defines $F$ to be the operator whose definition is obtained from the definition of $F$ in $M$ by replacing each declared constant or variable $p_{j}$ of $M$ with $e_{j}$. (If $m=0$, the WITH is omitted.)
$N\left(x_{1}, \ldots, x_{n}\right) \triangleq$ INSTANCE $M$ WITH $p_{1} \leftarrow e_{1}, \ldots, p_{m} \leftarrow e_{m}$

For each defined operator $F$ of module $M$, this defines $N\left(d_{1}, \ldots, d_{n}\right)!F$ to be the operator whose definition is obtained from the definition of $F$ by replacing each declared constant or variable $p_{j}$ of $M$ with $e_{j}$, and then replacing each identifier $x_{k}$ with $d_{k}$. (If $m=0$, the WITH is omitted.)

THEOREM $P$

Asserts that $P$ can be proved from the definitions and assumptions of the current module.

LOCAL def

Makes the definition(s) of def (which may be a definition or an INSTANCE statement) local to the current module, thereby not obtained when extending or instantiating the module.

Ends the current module or submodule.

## The Constant Operators

![](https://cdn.mathpix.com/cropped/2024_06_23_576f0b4b8da858775992g-4.jpg?height=224&width=813&top_left_y=124&top_left_x=157)

## Sets

$=\neq \in \cup$.

$\left\{e_{1}, \ldots, e_{n}\right\} \quad$ [Set consisting of elements $\left.e_{i}\right]$

$\{x \in S: p\}$ (2) [Set of elements $x$ in $S$ satisfying $p$ ]

$\{e: x \in S\}^{(1)} \quad$ [Set of elements $e$ such that $x$ in $\left.S\right]$

SUBSET $S \quad$ [Set of subsets of $S$ ]

UNION $S \quad[$ Union of all elements of $S]$

## Functions

$$
\begin{array}{ll}
f[e] & {[\text { Function application] }} \\
\text { DOMAIN } f & {[\text { Domain of function } f]} \\
{[x \in S \mapsto e]{ }^{(1)}} & {[\text { Function } f \text { such that } f[x]=e \text { for } x \in S]} \\
{[S \rightarrow T]} & {[\text { Set of functions } f \text { with } f[x] \in T \text { for } x \in} \\
{\left[f \text { EXCEPT }!\left[e_{1}\right]=e_{2}\right]^{(3)}} & {\left[\text { Function } \widehat{f} \text { equal to } f \text { except } \widehat{f}\left[e_{1}\right]=e_{2}\right]}
\end{array}
$$

## Records

$\begin{array}{ll}e . h & \text { [The } h \text {-field of record } e] \\ {\left[h_{1} \mapsto e_{1}, \ldots, h_{n} \mapsto e_{n}\right]} & \left.\text { [The record whose } h_{i} \text { field is } e_{i}\right] \\ {\left[h_{1}: S_{1}, \ldots, h_{n}: S_{n}\right]} & \left.\text { [Set of all records with } h_{i} \text { field in } S_{i}\right] \\ {[r \text { EXCEPT }!. h=e]} & \text { [3) } \\ \text { [Record } \widehat{r} \text { equal to } r \text { except } \widehat{r} . h=e]\end{array}$

## Tuples

$$
\begin{array}{ll}
e[i] & {\left[\text { The } i^{\text {th }} \text { component of tuple } e\right]} \\
\left\langle e_{1}, \ldots, e_{n}\right\rangle & {\left[\text { The } n \text {-tuple whose } i^{\text {th }} \text { component is } e_{i}\right]} \\
S_{1} \times \ldots \times S_{n} & \text { [The set of all } \left.n \text {-tuples with } i^{\text {th }} \text { component in } S_{i}\right]
\end{array}
$$

(1) $x \in S$ may be replaced by a comma-separated list of items $v \in S$, where $v$ is either a comma-separated list or a tuple of identifiers

(2) $x$ may be an identifier or tuple of identifiers.

(3)! $\left[e_{1}\right]$ or !.h may be replaced by a comma separated list of items $!a_{1} \cdots a_{n}$, where each $a_{i}$ is $\left[e_{i}\right]$ or.$h_{i}$.

## Miscellaneous Constructs

![](https://cdn.mathpix.com/cropped/2024_06_23_576f0b4b8da858775992g-5.jpg?height=416&width=1145&top_left_y=122&top_left_x=162)

## Action Operators

| $e^{\prime}$ | $[$ The value of $e$ in the final state of a step] |
| :--- | :--- |
| $[A]_{e}$ | $\left[A \vee\left(e^{\prime}=e\right)\right]$ |
| $\langle A\rangle_{e}$ | $\left[A \wedge\left(e^{\prime} \neq e\right)\right]$ |
| ENABLED $A$ | $[$ An step is possible $]$ |
| UNCHANGED $e$ | $\left[e^{\prime}=e\right]$ |
| $A \cdot B$ | $[$ Composition of actions $]$ |

## Temporal Operators

$$
\begin{array}{ll}
\square F & {[F \text { is always true }]} \\
\diamond F & {[F \text { is eventually true }]} \\
\mathrm{WF}_{e}(A) & {[\text { Weak fairness for action } A]} \\
\mathrm{SF}_{e}(A) & {[\text { Strong fairness for action } A]} \\
F \sim G & {[F \text { leads to } G]}
\end{array}
$$

## User-Definable Operator Symbols

## Infix Operators

| $+^{(1)}$ | $-{ }^{(1)}$ | $*$ | $(1)$ | $/{ }^{(2)}$ | $\circ{ }^{(3)}$ | ++ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $\div^{(1)}$ | $\%^{(1)}$ | - | $(1,4)$ | $\ldots$ |  |  |
| $\oplus^{(5)}$ | $\ominus{ }^{(5)}$ | $\otimes$ | $\oslash$ | $\odot$ | -- |  |
| (1) $^{(1)}$ | $>{ }^{(1)}$ | $\leq$ | ${ }^{(1)}$ | $\geq{ }^{(1)}$ | $\sqcap$ | $* *$ |
| $\prec$ | $\succ$ | $\preceq$ | $\succeq$ | $\sqcup$ | $\sim-$ |  |
| $\ll$ | $\gg$ | $<:$ | $:>^{(6)}$ | $\&$ | $\& \&$ |  |
| $\sqsubset$ | $\sqsupset$ | $\sqsubseteq{ }^{(5)}$ | $\sqsupseteq$ | $\mid$ | $\% \%$ |  |
| $\subset$ | $\supset$ |  | $\supseteq$ | $\star$ | $@ @(6)$ |  |
| $\vdash$ | $\dashv$ | $\models$ | $=$ | $\bullet$ | $\# \#$ |  |
| $\sim$ | $\simeq$ | $\approx$ | $\cong$ | $\$$ | $\$ \$$ |  |
| $\bigcirc$ | $::=$ | $\asymp$ | $\doteq$ | $? ?$ | $!!$ |  |
| $\propto$ | $\checkmark$ | $\uplus$ |  |  |  |  |

Postfix Operators (7)

$\sim \quad$ - $＃$

(1) Defined by the Naturals, Integers, and Reals modules.

(2) Defined by the Reals module.

(3) Defined by the Sequences module.

(4) $x^{\wedge} y$ is printed as $x^{y}$.

(5) Defined by the Bags module.

(6) Defined by the $T L C$ module.

(7) $e^{\wedge}+$ is printed as $e^{+}$, and similarly for ${ }^{\wedge} *$ and $\wedge \#$.

## Precedence Ranges of Operators

The relative precedence of two operators is unspecified if their ranges overlap. Left-associative operators are indicated by (a).

![](https://cdn.mathpix.com/cropped/2024_06_23_576f0b4b8da858775992g-7.jpg?height=1394&width=1163&top_left_y=291&top_left_x=176)[^0]

## Operators Defined in Standard Modules.

Modules Naturals, Integers, Reals

$$
\begin{aligned}
& +\quad-{ }^{(1)} \quad * \quad /^{(2)} \quad \sim^{-(3)} \quad \text {.. } \quad \text { Nat } \quad \text { Real }^{(2)} \\
& \div \quad \% \quad \geq \quad>\quad \text { Int }^{(4)} \quad \text { Infinity }{ }^{(2}
\end{aligned}
$$

(1) Only infix - is defined in Naturals.

(2) Defined only in Reals module.

(3) Exponentiation.

(4) Not defined in Naturals module.

Module Sequences

| $\circ$ | Head | SelectSeq | SubSeq |
| :--- | :--- | :--- | :--- |
| Append | Len | Seq | Tail |

## Module FiniteSets <br> IsFiniteSet Cardinality

Module Bags

| $\oplus$ | BagIn | CopiesIn | SubBag |
| :--- | :--- | :--- | :--- |
| $\ominus$ | BagOfAll | EmptyBag |  |
| $\sqsubseteq$ | BagToSet | IsABag |  |
| BagCardinality | BagUnion | SetToBag |  |

Module RealTime

RTBound RTnow now (declared to be a variable)

Module $T L C$

$:>$ Print Assert JavaTime Permutations
SortSeq

## ASCII Representation of Typeset Symbols

| $/$ or $\backslash]$ | land | V | $\backslash /$ or $\backslash$ lor | $\Rightarrow$ |  |
| :---: | :---: | :---: | :---: | :---: | :---: |
| $\sim$ or $\backslash \operatorname{lr}$ | not or \neg | $\equiv$ | $\Leftrightarrow$ or \equiv | $\triangleq$ | $==$ |
| \in |  | $\notin$ | \notin | $\neq$ | $\#$ or $/=$ |
| $<<$ |  | \rangle | $>$ | $\square$ | [] |
| $<$ |  | $>$ | $>$ | $\diamond$ | $<>$ |
| \leq or | $=<$ or $<=$ | $\geq$ | \geq or >= | $\sim$ | $\sim$ |
| \ll |  | $\gg$ | $\backslash g g$ | $\xrightarrow{\square}$ | $-+->$ |
| \prec |  | $\succ$ | \succ | $\mapsto$ | $\|-\rangle$ |
| \preceq |  | $\succeq$ | \succeq | $\div$ | \div |
| \subsete |  | $\supseteq$ | \supseteq | . | \cdot |
| \subset |  | $\supset$ | \supset | 0 | \o or \circ |
| \sqsubse |  | $\sqsupset$ | \sqsupset | $\bullet$ | \bullet |
| \sqsubse | eteq | $\sqsupseteq$ | \sqsupseteq | $\star$    | \star |
| $1-$ |  | $\dashv$ | -1 | O | \bigcirc |
| $\mid=$ |  | $=$ | $=1$ | $\sim$ | \sim |
| $\rightarrow$ |  | $\leftarrow$ | $<-$ | $\simeq$ | \simeq |
| \cap or | \intersect | $\cup$ | \cup or \union | $\asymp$ | \asymp |
| \sqcap |  | $\sqcup$ | \sqcup | $\approx$ | \approx |
| $(+)$ or 1 | \oplus | $\uplus$ | \uplus | $\cong$ | \cong |
| $(-)$ or 1 | \ominus | $\times$ | $\backslash X$ or \times | $\doteq$ | \doteq |
| (.) or 1 | \odot | 2 | $\backslash w r$ | $x^{y}$ | $x^{\wedge} y^{(2)}$ |
| $(\backslash X)$ or | \otimes | $\propto$ | \propto | $x^{+}$ | $\mathrm{x}^{\wedge}+(2)$ |
| (/) or 1 | \oslash | "s" | "s" (1) | $x^{*}$ | $\mathrm{x}^{\wedge} *{ }^{(2)}$ |
| $\backslash E$ |  | $\forall$ | $\backslash \mathrm{A}$ | $x^{\#}$ | $x^{\wedge} \#^{(2)}$ |
| $\backslash \mathrm{EE}$ |  | $\forall$ | $\backslash \mathrm{AA}$ | $\prime$ | , |
| ]_v |  | \rangle$_{v}$ | $>>\_v$ |  |  |
| $W F_{-} v$ |  | $\mathrm{SF}_{2}$ | SF_v |  |  |

(1) $s$ is a sequence of characters.

(2) $x$ and $y$ are any expressions.

(3) a sequence of four or more - or $=$ characters.


[^0]:    (1) Action composition (\cdot).

    (2) Record field (period).

