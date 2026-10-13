# Week 1 — Sets, Functions & Proof Techniques

**Course:** COT 4210 — Automata Theory & Formal Languages (USF Fall 2026)
**Lecture 1** · Topics: sets and set operations, functions and growth rates (Big-O / Big-Ω / Big-Θ), proof techniques (induction, strong induction, contradiction).
**Sections:** 1.1 Mathematical Preliminaries and Notation (PDF pp. 19–37).
**Introduction:** Theory-of-computation overview (PDF pp. 15–18).
**Source:** Peter Linz, *An Introduction to Formal Languages and Automata*, local 6th-edition PDF. Page references count PDF pages, not printed pages; the course lists the 7th edition.

---

## 1. Sets

The **theory of computation** studies abstract models of computers and the languages they process. **Computability** concerns whether a problem can be solved algorithmically at all; **complexity** concerns the resources needed to solve it. Languages describe sets of permitted strings, grammars generate those strings, and automata process them.

### 1.1 Definition

> **Definition (Set).** A *set* is a collection of elements with no structure other than membership.
> - Order does not matter: $\lbrace a, b\rbrace = \lbrace b, a\rbrace$.
> - Duplicates do not count more than once: $\lbrace a, a, b\rbrace = \lbrace a, b\rbrace$.

If $x$ is an element of set $S$, we write $x \in S$ (read as "x belongs to S"). The negation is written $x \notin S$.

### 1.2 Ways to Represent a Set

**1. Roster (listing) notation.** List the elements inside braces, using an ellipsis $\cdots$ when the pattern is clear:

$$
\lbrace a, b, \dots, z\rbrace = \text{all lowercase English letters}, \qquad \lbrace 2, 4, 6, \dots\rbrace = \text{positive even integers}.
$$

**2. Set-builder notation.** Describe a property that exactly the members satisfy:

$$
S = \lbrace \, i : i > 0,\ i \text{ is even}\,\rbrace
$$

read as "$S$ is the set of all $i$ such that $i > 0$ and $i$ is even" (with $i$ understood to range over integers). The general form is $\lbrace \, x : P(x)\,\rbrace$ — "all $x$ with property $P$."

**3. Venn diagrams.** Draw the universal set as a rectangle and each set as a closed region; overlaps show intersection. Useful for visualizing subset/union/complement relationships.

### 1.3 Set Properties

> **Definition (Cardinality).** The *cardinality* of a finite set $S$ is the number of elements it contains, written $|S|$. Duplicates are not counted: if $C = \lbrace a, a, b, b, b, c\rbrace$ then $|C| = 3$.

> **Definition (Empty Set).** The set with no elements is the *empty set* (or *null set*), written $\varnothing$ or $\lbrace \rbrace$. Note that $\varnothing \neq \lbrace \varnothing\rbrace$: the empty set contains nothing ($|\varnothing| = 0$), while $\lbrace \varnothing\rbrace$ contains exactly one element — namely the empty set itself ($|\lbrace \varnothing\rbrace| = 1$).

> **Definition (Subset).** $S_1$ is a *subset* of $S$, written $S_1 \subseteq S$, if every element of $S_1$ is also an element of $S$:

$$
S_1 \subseteq S \iff (\forall x)(x \in S_1 \Rightarrow x \in S).
$$

> **Definition (Proper Subset).** If $S_1 \subseteq S$ but $S$ contains at least one element not in $S_1$, then $S_1$ is a *proper subset* of $S$, written $S_1 \subset S$.

> **Definition (Equal Sets).** Two sets are equal iff they have exactly the same elements:

$$
A = B \iff A \subseteq B \text{ and } B \subseteq A.
$$

> **Definition (Disjoint Sets).** $S_1$ and $S_2$ are *disjoint* if they share no element, i.e. $S_1 \cap S_2 = \varnothing$.

A set is **finite** if it contains finitely many elements; otherwise it is **infinite**.

> **Definition (Powerset).** The *powerset* of $S$, written $\mathcal{P}(S)$ or $2^S$, is the set of all subsets of $S$ — a set of sets. If $|S| = n$, then

$$
|\mathcal{P}(S)| = 2^{n}.
$$

**Example.** For $S = \lbrace a, b, c\rbrace$:

$$
\mathcal{P}(S) = \left \lbrace \varnothing,\ \lbrace a\rbrace,\ \lbrace b\rbrace,\ \lbrace c\rbrace,\ \lbrace a,b\rbrace,\ \lbrace a,c\rbrace,\ \lbrace b,c\rbrace,\ \lbrace a,b,c\rbrace\right\rbrace
$$

so $|S| = 3$ and $|\mathcal{P}(S)| = 8 = 2^3$.

> **Definition (Cartesian Product).** For sets $S_1, S_2$, the *Cartesian product* is the set of all ordered pairs:

$$
S_1 \times S_2 = \lbrace \,(x, y) : x \in S_1,\ y \in S_2\,\rbrace.
$$

Order matters in a pair: $(4, 2)$ and $(2, 4)$ are different. For $n$ sets:

$$
S_1 \times S_2 \times \cdots \times S_n = \lbrace \,(x_1, x_2, \dots, x_n) : x_i \in S_i\,\rbrace.
$$

**Example.** With $S_1 = \lbrace 2, 4\rbrace$ and $S_2 = \lbrace 2, 3, 5, 6\rbrace$:

$$
S_1 \times S_2 = \lbrace (2,2),(2,3),(2,5),(2,6),(4,2),(4,3),(4,5),(4,6)\rbrace.
$$

$(4,2) \in S_1 \times S_2$ but $(2,4) \notin S_1 \times S_2$.

### 1.4 Set Operations

For sets $S_1, S_2$:

| Operation | Definition |
|---|---|
| **Union** | $S_1 \cup S_2 = \lbrace \, x : x \in S_1 \text{ or } x \in S_2\,\rbrace$ |
| **Intersection** | $S_1 \cap S_2 = \lbrace \, x : x \in S_1 \text{ and } x \in S_2\,\rbrace$ |
| **Difference** | $S_1 - S_2 = \lbrace \, x : x \in S_1 \text{ and } x \notin S_2\,\rbrace$ |

> **Definition (Complement).** Given a universal set $U$ of all possible elements under discussion, the *complement* of $S$ is

$$
\overline{S} = U - S = \lbrace \, x : x \in U \text{ and } x \notin S\,\rbrace.
$$

**Basic identities with $\varnothing$:**

$$
S \cup \varnothing = S, \qquad S - \varnothing = S, \qquad S \cap \varnothing = \varnothing, \qquad \overline{\varnothing} = U, \qquad \overline{\overline{S}} = S.
$$

> **Theorem (De Morgan's Laws).** For any sets $S_1, S_2$:

$$
\overline{S_1 \cup S_2} = \overline{S_1} \cap \overline{S_2}, \qquad \overline{S_1 \cap S_2} = \overline{S_1} \cup \overline{S_2}.
$$

These are needed repeatedly throughout the course (e.g., when manipulating regular expressions and language complements).

### 1.5 Partitions

> **Definition (Partition).** A partition of a set separates it into nonempty, mutually disjoint subsets whose union is the entire set.

Each element belongs to exactly one part. A partition groups elements without leaving any out or placing an element in two groups.

---

## 2. Functions and Growth Rates

### 2.1 Definition of a Function

> **Definition (Function).** A *function* is a rule that assigns to each element of one set exactly one element of another set. We write

$$
f : S_1 \to S_2
$$

> where $S_1$ is the **domain**, $S_2$ is the **codomain** (target), and $f(a)$ is the output assigned to input $a$. The *range* (or image) of $f$ is $\lbrace \, f(a) : a \in S_1\,\rbrace \subseteq S_2$.

- If the domain is all of $S_1$, $f$ is a **total function** on $S_1$; otherwise it is a **partial function**.
- In CS we often study functions whose inputs/outputs are positive integers and care only about behavior as the argument grows large — this motivates *order-of-magnitude* notation.

### 2.2 Dominant Term and Growth Rate

For $f(n) = 2n^2 + 3n$: ask "when $n$ becomes very large, which term matters most?" The $n^2$ term dominates the linear term — for asymptotic growth, constants and lower-order terms become negligible.

**Growth comparison.** Let $f(n) = 2n+1$, $g(n) = n^2$, $h(n) = 2^n$:

| $n$ | $2n + 1$ | $n^2$ | $2^n$ |
|---:|---:|---:|---:|
| 2 | 5 | 4 | 4 |
| 5 | 11 | 25 | 32 |
| 10 | 21 | 100 | 1,024 |
| 20 | 41 | 400 | 1,048,576 |

Linear $\ll$ quadratic $\ll$ exponential.

### 2.3 Asymptotic Notation

Let $f(n)$ and $g(n)$ be functions on a subset of the positive integers. All statements below must hold for all $n \ge n_0$ for some integer threshold $n_0$.

> **Definition (Big-O, upper bound).**

$$
f(n) = O(g(n)) \iff \exists\, c > 0,\ \exists\, n_0 : (\forall n \ge n_0)\ f(n) \le c\,|g(n)|.
$$

> *Interpretation:* after some point $n_0$, a constant multiple of $g$ stays **above** $f$. Big-O is a ceiling.

> **Definition (Big-Ω, lower bound).**

$$
f(n) = \Omega(g(n)) \iff \exists\, c > 0,\ \exists\, n_0 : (\forall n \ge n_0)\ f(n) \ge c\,|g(n)|.
$$

> *Interpretation:* after some point $n_0$, $f$ stays **above** a constant multiple of $g$. Big-Ω is a floor.

> **Definition (Big-Θ, tight bound).**

$$
f(n) = \Theta(g(n)) \iff \exists\, c_1 > 0,\ c_2 > 0,\ n_0 : (\forall n \ge n_0)\ c_1|g(n)| \le f(n) \le c_2|g(n)|.
$$

> *Interpretation:* $f$ is trapped between two constant multiples of $g$. Big-Θ is a sandwich.

**Mnemonic:** $O$ = ceiling, $\Omega$ = floor, $\Theta$ = sandwich. Note that $f = \Theta(g)$ iff $f = O(g)$ and $f = \Omega(g)$.

The equality sign in asymptotic notation describes a growth bound, not ordinary numerical equality. Do not cancel or subtract Big-O expressions as if they were numbers.

### 2.4 Relations and Equivalence Classes

> **Definition (Relation).** A relation from $A$ to $B$ is a subset of $A \times B$. Unlike a function, it can associate an input with several outputs or with none.

A function is a special relation with exactly one output for every input in its domain. For a partial function, some inputs in the stated input set may have no output.

> **Definition (Equivalence Relation).** A relation on a set is an equivalence relation if it is reflexive, symmetric, and transitive.

| Property | Requirement |
|---|---|
| Reflexive | Every element is related to itself. |
| Symmetric | If $x \equiv y$, then $y \equiv x$. |
| Transitive | If $x \equiv y$ and $y \equiv z$, then $x \equiv z$. |

An **equivalence class** contains all elements related to a given element. The distinct equivalence classes partition the set. For instance, grouping nonnegative integers by their remainder modulo three gives three classes; the relation compares remainders, not the integers themselves.

---

## 3. Graphs and Trees

> **Definition (Directed Graph).** A directed graph consists of vertices and directed edges. An edge $(u,v)$ goes from $u$ to $v$; it is outgoing from $u$ and incoming to $v$. Vertices and edges may have labels.

| Term | Meaning in this textbook |
|---|---|
| Walk | A sequence of connecting directed edges; repetitions are allowed. |
| Length of a walk | Number of edges traversed. |
| Path | A walk that does not repeat an edge. |
| Simple path | A path that does not repeat a vertex. |
| Cycle | A walk back to its starting vertex without repeating an edge. |
| Simple cycle | A cycle with no repeated vertex except the start/end. |
| Loop | An edge from a vertex to itself. |
| Label of a walk | The sequence of edge labels read along it. |

To enumerate simple paths, extend a partial path along outgoing edges, but do not revisit a vertex already on it. In a finite graph this process terminates. Graph terminology varies between texts; the table uses Linz's conventions.

> **Definition (Rooted Tree).** A rooted tree is an acyclic directed graph with a distinguished root and exactly one path from that root to every other vertex.

- The **root** has no incoming edge; a **leaf** has no outgoing edge.
- An edge from $u$ to $v$ makes $u$ the **parent** and $v$ its **child**.
- A vertex's **level** is the number of edges from the root; the root has level zero.
- The **height** is the largest vertex level.
- An **ordered tree** fixes an order among children; a **binary tree** allows at most two children per vertex.

---

## 4. Proof Techniques

### 4.1 Direct Reasoning

A direct proof starts from definitions, hypotheses, and known facts, then derives the conclusion using valid logical steps. To prove two sets or languages equal, prove both inclusions: everything in the first is in the second, and everything in the second is in the first.

### 4.2 Mathematical Induction

To prove a statement $P(n)$ for all integers $n \ge n_{\text{base}}$:

1. **Base case.** Show $P(n_{\text{base}})$ is true (usually $n = 0$ or $n = 1$).
2. **Inductive step.** *Assume* $P(k)$ for some arbitrary $k \ge n_{\text{base}}$ (the inductive hypothesis), and use it to prove $P(k+1)$.

By induction, $P(n)$ holds for all $n \ge n_{\text{base}}$.

### 4.3 Strong Induction

Same base case, but the inductive step assumes **all** cases from the starting index through $k$ are true and proves $P(k+1)$. Use it when proving $P(k+1)$ requires more than just $P(k)$.

### 4.4 Proof by Contradiction

To prove a claim: assume the **negation** of the claim, derive a logical contradiction (a statement that is both true and false, or contradicts a known fact), and conclude the original claim must be true.

Induction matches recursion: the base case supplies a starting value, and the inductive step explains how a larger case follows from smaller cases. When proving a property of strings or derivations, their length or number of steps is often the induction parameter.

---

## Quick Reference

| Symbol | Meaning |
|---|---|
| $x \in S$, $x \notin S$ | membership / non-membership |
| $\varnothing$ or $\lbrace \rbrace$ | empty set |
| $A \subseteq B$, $A \subset B$ | subset / proper subset |
| $A = B$ | iff $A \subseteq B$ and $B \subseteq A$ |
| $A \cup B$, $A \cap B$, $A - B$ | union, intersection, difference |
| $\overline{A}$ | complement (relative to universal set $U$) |
| $\mathcal{P}(S)$ or $2^S$ | powerset; $|\mathcal{P}(S)| = 2^{|S|}$ |
| $A \times B$ | Cartesian product of ordered pairs |
| $f : A \to B$ | function from domain $A$ to codomain $B$ |
| $O(g)$, $\Omega(g)$, $\Theta(g)$ | upper bound / lower bound / tight bound |
| partition | nonempty, disjoint parts covering a set |
| equivalence relation | reflexive, symmetric, transitive relation |
| equivalence class | all elements related to one element; classes partition the set |
| walk / simple path | connected edge sequence / no repeated vertices |
| rooted tree | acyclic graph with one root-to-vertex path |
| level / height | distance from root / largest level |
| induction | base case, hypothesis, inductive step |
| contradiction | assume the negation and derive an impossibility |
