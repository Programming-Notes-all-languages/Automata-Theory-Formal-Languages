# Weeks 4–5 — Deterministic Finite Automata

**Course:** COT 4210 — Automata Theory & Formal Languages (USF Fall 2026)
**Lectures 4–5** · Topics: deterministic finite automata, transition graphs, accepted languages, regular languages.

---

## 1. Deterministic Finite Automata

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

The formal tuple and its transition graph describe the same automaton.

### 1.2 Extended Transition Function

> **Definition (Extended Transition Function).** The extended transition function $\delta^{*} : Q \times \Sigma^{*} \to Q$ gives the state reached after reading a whole string. It is defined recursively by

$$
\delta^{*}(q, \lambda) = q,
$$

$$
\delta^{*}(q, wa) = \delta(\delta^{*}(q, w), a),
$$

> for every $q \in Q$, $w \in \Sigma^{*}$, and $a \in \Sigma$.

The base case says that reading no input leaves the automaton in its current state. The recursive case processes the final symbol after the prefix $w$.

---

## 2. Languages Accepted by DFAs

> **Definition (Language Accepted by a DFA).** The language accepted by $M = (Q, \Sigma, \delta, q_0, F)$ is

$$
L(M) = \lbrace w \in \Sigma^{*} : \delta^{*}(q_0, w) \in F \rbrace.
$$

Its complement with respect to $\Sigma^{*}$ is

$$
\overline{L(M)} = \lbrace w \in \Sigma^{*} : \delta^{*}(q_0, w) \notin F \rbrace.
$$

> **Definition (Regular Language).** A language $L$ is *regular* if and only if there is a DFA $M$ such that $L = L(M)$.

> **Theorem (Closure Under Complement).** If $L$ is regular, then $\overline{L}$ is regular. For a DFA, complementing the language is achieved by exchanging final and nonfinal states while preserving the complete transition function.

A DFA has only finitely many states, so it can record only finitely many distinct situations about the input read so far. Languages that require unbounded information cannot be recognized by a DFA.

---

## Quick Reference

| Term / Symbol | Meaning |
|---|---|
| $M = (Q, \Sigma, \delta, q_0, F)$ | DFA: states, alphabet, transition function, initial state, final states |
| $\delta : Q \times \Sigma \to Q$ | total one-symbol transition function |
| $\delta(q, a)$ | state reached from $q$ after reading symbol $a$ |
| $q_0$ | initial state |
| $F$ | set of final (accepting) states |
| $\delta^{*} : Q \times \Sigma^{*} \to Q$ | extended transition function for whole strings |
| $\delta^{*}(q, \lambda) = q$ | no input leaves the current state unchanged |
| $L(M)$ | strings that leave $M$ in a final state |
| regular language | language accepted by some DFA |
| $\overline{L}$ | complement relative to $\Sigma^{*}$ |
