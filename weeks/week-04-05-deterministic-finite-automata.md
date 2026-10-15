# Weeks 4–5 — Deterministic Finite Automata

**Course:** COT 4210 — Automata Theory & Formal Languages (USF Fall 2026)
**Lectures 4–5** · Topics: deterministic finite automata, transition graphs, accepted languages, regular languages.
**Sections:** 2.1 Deterministic Finite Accepters (PDF pp. 76–90).
**Source:** Peter Linz, *An Introduction to Formal Languages and Automata*, **6th ed.**, local `COT 4210.pdf`. Page numbers refer to PDF pages, not printed pages; the course lists the 7th edition.

> **Study scope.** Concept-focused private study aids, not assignment solutions or submitted work.

---

## 1. Deterministic Finite Automata

A finite accepter recognizes a pattern by keeping only the information needed to decide acceptance. Its current state is its entire memory: it has no separate working storage and cannot rewrite or reread the input.

> **Definition (Deterministic Finite Accepter).** A deterministic finite accepter (DFA) is a quintuple

$$
M = (Q, \Sigma, \delta, q_0, F),
$$

> where $Q$ is a finite set of states, $\Sigma$ is a finite input alphabet, $\delta : Q \times \Sigma \to Q$ is a total transition function, $q_0 \in Q$ is the initial state, and $F \subseteq Q$ is the set of final states.

A DFA begins in $q_0$ and reads its input from left to right, consuming exactly one input symbol on each move. After the entire input has been read, it accepts exactly when its current state is in $F$; otherwise, it rejects.

**Deterministic** means that for each state $q \in Q$ and symbol $a \in \Sigma$, $\delta(q, a)$ specifies exactly one next state. Because $\delta$ is total, every state has a defined transition for every input symbol.

### 1.1 Transition Graphs

A **transition graph** is the visual representation of a DFA:

- Each vertex represents one state in $Q$.
- An edge from $q_i$ to $q_j$ labeled $a$ represents $\delta(q_i, a) = q_j$.
- An incoming unlabeled arrow marks the initial state.
- A double circle marks each final state.

The formal tuple and its transition graph describe the same automaton. Several symbols on one edge are shorthand for separate transitions with the same destination; they are alternatives, not a string to read at once. Each state still needs exactly one outgoing transition for each alphabet symbol.

A **transition table** is another representation of $\delta$: a row selects the current state, a column selects the input symbol, and the cell gives the next state. A table-driven simulation updates the state once per symbol and tests final-state membership only after the input ends.

> **Definition (Trap State).** A trap state $t$ has $\delta(t, a) = t$ for every $a \in \Sigma$. Once entered, it cannot be left.

A nonfinal trap permanently rejects; a final trap permanently accepts every continuation. A missing DFA transition must not be interpreted as an unspecified choice: complete it, usually with a nonfinal trap.

### 1.2 Extended Transition Function

> **Definition (Extended Transition Function).** The extended transition function $\delta^{\ast} : Q \times \Sigma^{\ast} \to Q$ gives the state reached after reading a whole string. It is defined recursively by

$$
\delta^{\ast}(q, \lambda) = q,
$$

$$
\delta^{\ast}(q, wa) = \delta(\delta^{\ast}(q, w), a),
$$

> for every $q \in Q$, $w \in \Sigma^{\ast}$, and $a \in \Sigma$.

The base case says that reading no input leaves the automaton in its current state. The recursive case processes the final symbol after the prefix $w$.

> **Theorem (Walk Interpretation).** For a nonempty string $w$, $\delta^{\ast}(p, w) = q$ exactly when the transition graph has a walk from $p$ to $q$ labeled $w$.

Concatenate the edge labels in traversal order to obtain the walk's label. States and edges may repeat; a walk need not be a simple path. For $\lambda$, the zero-edge walk leaves the state unchanged. The recursive transition rule and this graph interpretation agree by induction on input length.

---

## 2. Languages Accepted by DFAs

> **Definition (Language Accepted by a DFA).** The language accepted by $M = (Q, \Sigma, \delta, q_0, F)$ is

$$
L(M) = \lbrace w \in \Sigma^{\ast} : \delta^{\ast}(q_0, w) \in F \rbrace.
$$

Its complement with respect to $\Sigma^{\ast}$ is

$$
\overline{L(M)} = \lbrace w \in \Sigma^{\ast} : \delta^{\ast}(q_0, w) \notin F \rbrace.
$$

> **Definition (Regular Language).** A language $L$ is *regular* if and only if there is a DFA $M$ such that $L = L(M)$.

In particular, $\lambda$ is accepted exactly when $q_0 \in F$. Reaching a final state partway through a nonempty input does **not** end the computation; a later symbol can move the automaton to a nonfinal state. Final states need not be traps.

> **Theorem (Closure Under Complement).** If $L$ is regular, then $\overline{L}$ is regular. For a DFA, complementing the language is achieved by exchanging final and nonfinal states while preserving the complete transition function.

A DFA has only finitely many states, so it can record only finitely many distinct situations about the input read so far. It may process arbitrarily long strings, but it cannot retain an unbounded amount of information about them.

### 2.1 Designing States as Memory

Give each state a clear **meaning about the prefix already read**, then choose transitions that preserve that meaning. State names are arbitrary; mnemonic labels help explain why the transitions work.

| Recognition task | Information a state retains |
|---|---|
| Required prefix | How much of the prefix matched, or whether it already failed |
| Required or forbidden substring | Relevant suffix of the input so far, plus whether the pattern occurred |
| Required ending | Enough recent input to decide whether the current prefix ends correctly |
| Irreversible condition | A trap once later input can no longer change the decision |

Specify the initial state's meaning before any input, every state-symbol transition, and which meanings imply acceptance at end of input. Check $\lambda$, short strings, and overlapping partial matches. Testing a few strings can reveal errors, but correctness requires the state meanings to hold for **every** input prefix.

To establish regularity, construct an accepter and explain both directions: every string in the intended language is accepted, and every accepted string meets the intended condition. One language may have many different accepting DFAs; a construction need not be minimal to show regularity.

---

### 2.2 Midterm Practice — Define a DFA from Its Diagram

**Source:** *Practice Exam 1*, question 2 (3 points), supplied practice material rather than a textbook exercise.

<details>
<summary>Question 2 — Define the DFA and give five accepted strings</summary>

Define a DFA for the supplied state diagram. Then list at least five strings of length 5 that it accepts. The diagram's states are $q_0,q_1,q_2,q_3$; its start arrow points to $q_0$, and only $q_3$ has a double circle. The transition table in the solution transcribes every labeled arrow in that diagram.

<details>
<summary>Solution</summary>

**Define all five components.** A transition table specifies the transition function, but a complete formal definition also identifies the states, alphabet, start state, and accepting states.

| Component | Definition |
|---|---|
| States | $Q = \lbrace q_0,q_1,q_2,q_3 \rbrace$ |
| Input alphabet | $\Sigma = \lbrace 0,1 \rbrace$ |
| Transition function | $\delta$, defined by the table below |
| Start state | $q_0$ |
| Accepting states | $F = \lbrace q_3 \rbrace$ |

Together these specify the DFA $M = (Q,\Sigma,\delta,q_0,F)$.

**Read each table row from the outgoing arrows:** from that state, follow the arrow labeled 0 or 1 to its destination.

| Current state | Read 0 | Read 1 |
|---|---|---|
| $q_0$ | $q_1$ | $q_3$ |
| $q_1$ | $q_1$ | $q_2$ |
| $q_2$ | $q_3$ | $q_2$ |
| $q_3$ | $q_3$ | $q_3$ |

**Find accepted strings.** Begin at $q_0$, consume every input symbol, and check whether the final state is $q_3$. Any string beginning with 1 reaches $q_3$ immediately; its loops on both symbols keep every continuation there. Strings beginning with 0 can also reach $q_3$ through $q_1$ and $q_2$.

Five distinct accepted strings of length 5 are:

| String | Why it is accepted |
|---|---|
| `10000` | The first 1 reaches the accepting state; the remaining symbols loop there. |
| `10101` | The first 1 reaches the accepting state; the remaining symbols loop there. |
| `11111` | The first 1 reaches the accepting state; the remaining symbols loop there. |
| `01000` | The prefix `010` reaches the accepting state; the remaining zeros loop there. |
| `00110` | The final 0 moves from $q_2$ into the accepting state. |

For example, trace `01000` one symbol at a time:

| Input consumed | Current state |
|---|---|
| None | $q_0$ |
| `0` | $q_1$ |
| `01` | $q_2$ |
| `010` | $q_3$ |
| `0100` | $q_3$ |
| `01000` | $q_3$ |

**What to write on the exam:** the five components (using the transition table to define the transition function) and five accepted length-5 strings. Redrawing the supplied diagram or tracing every example is not required by this prompt; the trace above explains how to check your strings.

</details>
</details>

---

## Quick Reference

| Term / Symbol | Meaning |
|---|---|
| $M = (Q, \Sigma, \delta, q_0, F)$ | DFA: states, alphabet, transition function, initial state, final states |
| $\delta : Q \times \Sigma \to Q$ | total one-symbol transition function |
| $\delta(q, a)$ | state reached from $q$ after reading symbol $a$ |
| transition graph / table | equivalent visual / tabular representations of $\delta$ |
| trap state | loops to itself on every input symbol; may be final or nonfinal |
| $q_0$ | initial state |
| $F$ | set of final (accepting) states |
| $\delta^{\ast} : Q \times \Sigma^{\ast} \to Q$ | extended transition function for whole strings |
| $\delta^{\ast}(q, \lambda) = q$ | no input leaves the current state unchanged |
| walk labeled $w$ | edge labels concatenate to $w$; repetition is allowed |
| $L(M)$ | strings that leave $M$ in a final state |
| acceptance of $\lambda$ | exactly when $q_0 \in F$ |
| state meaning | finite information retained about the prefix read so far |
| regular language | language accepted by some DFA |
| $\overline{L}$ | complement relative to $\Sigma^{\ast}$ |
