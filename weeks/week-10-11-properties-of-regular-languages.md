# Weeks 10–11 — Properties of Regular Languages

**Course:** COT 4210 — Automata Theory & Formal Languages (USF Fall 2026)
**Sections:** 4.1 Closure Properties of Regular Languages (PDF pp. 182–197).
**Source:** Peter Linz, *An Introduction to Formal Languages and Automata*, local 6th-edition PDF. Page references count PDF pages, not printed pages; the course lists the 7th edition.

---

## 1. What Closure Means

> **Definition (Closure).** A language family is closed under an operation when applying that operation to languages in the family always produces another language in the same family.

For regular languages, a closure proof explains how to combine or modify finite automata or regular expressions. Showing that one particular result is regular is not enough: the construction must work for every allowed input language.

Unless another alphabet is specified, set operations use languages over the same alphabet $\Sigma$, and complement is relative to $\Sigma^{\ast}$.

---

## 2. Simple Set Operations

> **Theorem (Regular-Language Closure).** Regular languages are closed under union, intersection, concatenation, complement, and star-closure.

| Operation | Construction idea |
|---|---|
| Union | Combine the regular expressions with $+$, or branch between their NFAs. |
| Intersection | Run two DFAs together and accept only when both accept. |
| Concatenation | Concatenate the expressions, or connect the first NFA to the second using empty-string transitions. |
| Complement | Exchange accepting and nonaccepting states in a complete DFA. |
| Star-closure | Apply star to the expression, or allow zero or more trips through its NFA. |

### 2.1 Union, Concatenation, and Star

If $r_1$ and $r_2$ describe regular languages $L_1$ and $L_2$, then

$$
L(r_1+r_2) = L_1 \cup L_2,
\qquad
L(r_1r_2) = L_1L_2,
\qquad
L(r_1^{\ast}) = L_1^{\ast}.
$$

These results follow directly from the definition of regular expressions. Positive closure also preserves regularity because it is concatenation with star:

$$
L^+ = LL^{\ast}.
$$

### 2.2 Complement

For a DFA $M$ accepting $L$, keep its states, start state, alphabet, and transitions, but replace its final-state set $F$ with $Q-F$:

$$
M = (Q,\Sigma,\delta,q_0,F),
\qquad
M' = (Q,\Sigma,\delta,q_0,Q-F).
$$

Then $M'$ accepts the complement of $L$.

**The DFA must be complete.** If a diagram omits transitions, first supply a rejecting trap state and all missing edges. Every input must finish in exactly one state for exchanging final and nonfinal states to work. Simply flipping an NFA's final states does not complement its language, because NFA acceptance asks whether at least one path accepts.

### 2.3 Product Construction

A product DFA remembers a pair of states: one from each original DFA. Both machines read the same symbol at each step.

Given

$$
M_1=(Q_1,\Sigma,\delta_1,s_1,F_1),
\qquad
M_2=(Q_2,\Sigma,\delta_2,s_2,F_2),
$$

use state set $Q_1\times Q_2$, start state $(s_1,s_2)$, and transition rule

$$
\delta((p,q),a) = (\delta_1(p,a),\delta_2(q,a)).
$$

After reading a string $w$, the pair records the state reached by each machine:

$$
\delta^{\ast}((s_1,s_2),w)
=
(\delta_1^{\ast}(s_1,w),\delta_2^{\ast}(s_2,w)).
$$

Only the final-state condition changes with the desired operation:

| Operation | A pair $(p,q)$ accepts when |
|---|---|
| Intersection | $p\in F_1$ **and** $q\in F_2$. |
| Union | $p\in F_1$ **or** $q\in F_2$. |
| Difference | $p\in F_1$ **and** $q\notin F_2$. |

The full product has $|Q_1||Q_2|$ states, although some may be unreachable. The construction is finite, so the result is regular.

### 2.4 Intersection and Difference through Set Identities

Intersection can also be expressed using only union and complement:

$$
L_1\cap L_2
=
\overline{\overline{L_1}\cup\overline{L_2}}.
$$

Difference is intersection with a complement:

$$
L_1-L_2=L_1\cap\overline{L_2}.
$$

Since the operations on the right preserve regularity, the operations on the left do as well. A constructive proof supplies an automaton; a closure argument can establish the result without drawing one.

---

## 3. Reversal

> **Theorem (Reversal).** If $L$ is regular, then its reversal $L^R$ is regular.

Reversal changes the order of the symbols in each string, not which strings are in the original language:

$$
L^R=\lbrace w^R : w\in L \rbrace.
$$

To reverse an automaton:

1. Reverse every transition edge, keeping its label.
2. Make the old initial state the only final state.
3. Add a new initial state with a $\lambda$-transition to every old final state.

An accepting walk is now traversed in the opposite direction, so its label is reversed. The result is an NFA, which is sufficient because NFAs and DFAs recognize the same language family. The new start state handles multiple old final states and preserves empty-string acceptance.

---

## 4. Homomorphisms

A homomorphism consistently replaces each input symbol with a fixed output string. It may expand a symbol into several symbols or erase it by replacing it with the empty string.

> **Definition (Homomorphism).** For alphabets $\Sigma$ and $\Gamma$, a homomorphism starts with a symbol mapping

$$
h:\Sigma\to\Gamma^{\ast}.
$$

Extend the mapping to strings by concatenating the images in the original order:

$$
h(a_1a_2\cdots a_n)=h(a_1)h(a_2)\cdots h(a_n),
\qquad
h(\lambda)=\lambda.
$$

The **homomorphic image** of a language is

$$
h(L)=\lbrace h(w):w\in L \rbrace.
$$

> **Theorem (Homomorphism).** The homomorphic image of a regular language is regular.

Given a regular expression for $L$, replace each alphabet symbol $a$ with an expression for its fixed string $h(a)$. Keep the union, concatenation, and star operators, adding parentheses where needed. If the mapping erases a symbol, substitute $\lambda$, not $\varnothing$.

---

## 5. Right Quotient

Right quotient removes a qualifying suffix. It is a language operation, not numerical division and not a general inverse of concatenation.

> **Definition (Right Quotient).** For languages over the same alphabet,

$$
L_1/L_2
=
\lbrace x\in\Sigma^{\ast}:xy\in L_1\text{ for some }y\in L_2 \rbrace.
$$

A prefix $x$ qualifies when **at least one** suffix from $L_2$ completes it into a string of $L_1$. Different prefixes may use different suffixes. The suffix need not have a fixed length.

> **Theorem (Right Quotient).** If $L_1$ and $L_2$ are regular, then $L_1/L_2$ is regular.

### 5.1 Automaton Construction

Start with a DFA for $L_1$. Retain its start state, transitions, and state set. Change only which states accept:

$$
F'
=
\lbrace q\in Q:
\delta^{\ast}(q,y)\in F\text{ for some }y\in L_2
\rbrace.
$$

A state becomes final if some suffix from $L_2$ can take it to an old final state. To determine this for each $q$:

1. Use the original DFA with $q$ as its temporary initial state.
2. Form its product with a DFA for $L_2$, accepting when both components accept.
3. Check whether a final pair is reachable from the product's initial pair, including by a zero-length walk.
4. Put $q$ in $F'$ exactly when such a pair is reachable.

The resulting DFA accepts $x$ precisely when its reached state has a qualifying continuation. The connection between prefix and suffix processing is

$$
\delta^{\ast}(q_0,xy)
=
\delta^{\ast}(\delta^{\ast}(q_0,x),y).
$$

If $\lambda\in L_2$, an old accepting state qualifies without reading any additional symbols. If $L_2$ is empty, no state qualifies and the quotient is empty.

---

## Quick Reference

| Term / Operation | Meaning or construction |
|---|---|
| closure | an operation always stays inside the language family |
| union / concatenation / star | build with regular-expression operators |
| complement | flip final states in a complete DFA |
| product DFA | track both original states on the same input |
| intersection | product accepts when both components accept |
| difference | first component accepts and second rejects |
| reversal | reverse edges and exchange start/final roles using an NFA |
| homomorphism | replace each symbol by a fixed string; regularity is preserved |
| right quotient | prefixes that can be completed using a suffix from the second language |
| quotient final states | states with a qualifying continuation to an old final state |
