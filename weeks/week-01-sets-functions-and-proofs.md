# Week 1 — Sets, Functions & Proof Techniques

**Course:** COT 4210 — Automata Theory & Formal Languages (USF Fall 2026)
**Lecture 1** · Topics: sets and set operations, functions and growth rates (Big-O / Big-Ω / Big-Θ), proof techniques (induction, strong induction, contradiction).

---

## 1. Sets

### 1.1 Definition

> **Definition (Set).** A *set* is a collection of elements with no structure other than membership.
> - Order does not matter: $\{a, b\} = \{b, a\}$.
> - Duplicates do not count more than once: $\{a, a, b\} = \{a, b\}$.

If $x$ is an element of set $S$, we write $x \in S$ ("$x$ belongs to $S$"). The negation is written $x \notin S$.

### 1.2 Ways to Represent a Set

**1. Roster (listing) notation.** List the elements inside braces, using an ellipsis $\cdots$ when the pattern is clear:

$$\{a, b, \dots, z\} = \text{all lowercase English letters}, \qquad \{2, 4, 6, \dots\} = \text{positive even integers}.$$

**2. Set-builder notation.** Describe a property that exactly the members satisfy:

$$S = \{\, i : i > 0,\ i \text{ is even}\,\}$$

read as "$S$ is the set of all $i$ such that $i > 0$ and $i$ is even" (with $i$ understood to range over integers). The general form is $\{\, x : P(x)\,\}$ — "all $x$ with property $P$."

**3. Venn diagrams.** Draw the universal set as a rectangle and each set as a closed region; overlaps show intersection. Useful for visualizing subset/union/complement relationships.

### 1.3 Set Properties

> **Definition (Cardinality).** The *cardinality* of a finite set $S$ is the number of elements it contains, written $|S|$. Duplicates are not counted: if $C = \{a, a, b, b, b, c\}$ then $|C| = 3$.

> **Definition (Empty Set).** The set with no elements is the *empty set* (or *null set*), written $\varnothing$ or $\{\}$. Note that $\varnothing \neq \{\varnothing\}$: the empty set contains nothing ($|\varnothing| = 0$), while $\{\varnothing\}$ contains exactly one element — namely the empty set itself ($|\{\varnothing\}| = 1$).

> **Definition (Subset).** $S_1$ is a *subset* of $S$, written $S_1 \subseteq S$, if every element of $S_1$ is also an element of $S$:
> $$S_1 \subseteq S \iff (\forall x)(x \in S_1 \Rightarrow x \in S).$$

> **Definition (Proper Subset).** If $S_1 \subseteq S$ but $S$ contains at least one element not in $S_1$, then $S_1$ is a *proper subset* of $S$, written $S_1 \subset S$.

> **Definition (Equal Sets).** Two sets are equal iff they have exactly the same elements:
> $$A = B \iff A \subseteq B \text{ and } B \subseteq A.$$

> **Definition (Disjoint Sets).** $S_1$ and $S_2$ are *disjoint* if they share no element, i.e. $S_1 \cap S_2 = \varnothing$.

A set is **finite** if it contains finitely many elements; otherwise it is **infinite**.

> **Definition (Powerset).** The *powerset* of $S$, written $\mathcal{P}(S)$ or $2^S$, is the set of all subsets of $S$ — a set of sets. If $|S| = n$, then
> $$|\mathcal{P}(S)| = 2^{n}.$$

**Example.** For $S = \{a, b, c\}$:

$$\mathcal{P}(S) = \left\{\varnothing,\ \{a\},\ \{b\},\ \{c\},\ \{a,b\},\ \{a,c\},\ \{b,c\},\ \{a,b,c\}\right\}$$

so $|S| = 3$ and $|\mathcal{P}(S)| = 8 = 2^3$.

> **Definition (Cartesian Product).** For sets $S_1, S_2$, the *Cartesian product* is the set of all ordered pairs:
> $$S_1 \times S_2 = \{\,(x, y) : x \in S_1,\ y \in S_2\,\}.$$

Order matters in a pair: $(4, 2)$ and $(2, 4)$ are different. For $n$ sets:

$$S_1 \times S_2 \times \cdots \times S_n = \{\,(x_1, x_2, \dots, x_n) : x_i \in S_i\,\}.$$

**Example.** With $S_1 = \{2, 4\}$ and $S_2 = \{2, 3, 5, 6\}$:

$$S_1 \times S_2 = \{(2,2),(2,3),(2,5),(2,6),(4,2),(4,3),(4,5),(4,6)\}.$$

$(4,2) \in S_1 \times S_2$ but $(2,4) \notin S_1 \times S_2$.

### 1.4 Set Operations

For sets $S_1, S_2$:

| Operation | Definition |
|---|---|
| **Union** | $S_1 \cup S_2 = \{\, x : x \in S_1 \text{ or } x \in S_2\,\}$ |
| **Intersection** | $S_1 \cap S_2 = \{\, x : x \in S_1 \text{ and } x \in S_2\,\}$ |
| **Difference** | $S_1 - S_2 = \{\, x : x \in S_1 \text{ and } x \notin S_2\,\}$ |

> **Definition (Complement).** Given a universal set $U$ of all possible elements under discussion, the *complement* of $S$ is
> $$\overline{S} = U - S = \{\, x : x \in U \text{ and } x \notin S\,\}.$$

**Basic identities with $\varnothing$:**

$$S \cup \varnothing = S, \qquad S - \varnothing = S, \qquad S \cap \varnothing = \varnothing, \qquad \overline{\varnothing} = U, \qquad \overline{\overline{S}} = S.$$

> **Theorem (De Morgan's Laws).** For any sets $S_1, S_2$:
> $$\overline{S_1 \cup S_2} = \overline{S_1} \cap \overline{S_2}, \qquad \overline{S_1 \cap S_2} = \overline{S_1} \cup \overline{S_2}.$$

These are needed repeatedly throughout the course (e.g., when manipulating regular expressions and language complements).

---

## 2. Functions and Growth Rates

### 2.1 Definition of a Function

> **Definition (Function).** A *function* is a rule that assigns to each element of one set exactly one element of another set. We write
> $$f : S_1 \to S_2$$
> where $S_1$ is the **domain**, $S_2$ is the **codomain** (target), and $f(a)$ is the output assigned to input $a$. The *range* (or image) of $f$ is $\{\, f(a) : a \in S_1\,\} \subseteq S_2$.

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
> $$f(n) = O(g(n)) \iff \exists\, c > 0,\ \exists\, n_0 : (\forall n \ge n_0)\ f(n) \le c\,|g(n)|.$$
> *Interpretation:* after some point $n_0$, a constant multiple of $g$ stays **above** $f$. Big-O is a ceiling.

> **Definition (Big-Ω, lower bound).**
> $$f(n) = \Omega(g(n)) \iff \exists\, c > 0,\ \exists\, n_0 : (\forall n \ge n_0)\ f(n) \ge c\,|g(n)|.$$
> *Interpretation:* after some point $n_0$, $f$ stays **above** a constant multiple of $g$. Big-Ω is a floor.

> **Definition (Big-Θ, tight bound).**
> $$f(n) = \Theta(g(n)) \iff \exists\, c_1 > 0,\ c_2 > 0,\ n_0 : (\forall n \ge n_0)\ c_1|g(n)| \le f(n) \le c_2|g(n)|.$$
> *Interpretation:* $f$ is trapped between two constant multiples of $g$. Big-Θ is a sandwich.

**Mnemonic:** $O$ = ceiling, $\Omega$ = floor, $\Theta$ = sandwich. Note that $f = \Theta(g)$ iff $f = O(g)$ and $f = \Omega(g)$.

---

## 3. Proof Techniques

### 3.1 Mathematical Induction

To prove a statement $P(n)$ for all integers $n \ge n_{\text{base}}$:

1. **Base case.** Show $P(n_{\text{base}})$ is true (usually $n = 0$ or $n = 1$).
2. **Inductive step.** *Assume* $P(k)$ for some arbitrary $k \ge n_{\text{base}}$ (the inductive hypothesis), and use it to prove $P(k+1)$.

By induction, $P(n)$ holds for all $n \ge n_{\text{base}}$.

### 3.2 Strong Induction

Same base case, but the inductive step assumes $P(0), P(1), \dots, P(k)$ are **all** true (i.e., $P(i)$ for every $i \le k$) and proves $P(k+1)$. Use it when proving $P(k+1)$ requires more than just $P(k)$.

### 3.3 Proof by Contradiction

To prove a claim: assume the **negation** of the claim, derive a logical contradiction (a statement that is both true and false, or contradicts a known fact), and conclude the original claim must be true.

---

## 4. Solved Problems from Class

### Problem 1 — Set-builder notation

Write $B = \{1, 4, 9, 16, 25, 36, 49, 64, 81, 100\}$ in set-builder notation.

**Solution.** Each element is a perfect square from $1^2$ to $10^2$:

$$B = \{\, k^2 : k \in \mathbb{N},\ 1 \le k \le 10\,\}.$$

### Problem 2 — Expanding set-builder notation

List all elements of $B = \{\, 2k + 1 : k \in \mathbb{N},\ 0 \le k \le 4\,\}$.

**Solution.** Substitute $k = 0, 1, 2, 3, 4$:

$$B = \{1, 3, 5, 7, 9\}.$$

### Problem 3 — Cardinality with duplicates

Let $C = \{a, a, b, b, b, c\}$. What is $|C|$?

**Solution.** Duplicates are not counted: $C = \{a, b, c\}$, so $|C| = 3$.

### Problem 4 — $\varnothing$ vs. $\{\varnothing\}$

What is the difference between $\varnothing$ and $\{\varnothing\}$?

**Solution.** $\varnothing$ contains no elements at all ($|\varnothing| = 0$). The set $\{\varnothing\}$ has exactly one element, which happens to be the empty set itself ($|\{\varnothing\}| = 1$). So $\varnothing \neq \{\varnothing\}$ and in fact $\varnothing \subset \{\varnothing\}$.

### Problem 5 — All subsets of a 3-element set

Write all subsets of $S = \{1, 2, 3\}$.

**Solution.** There are $2^{|S|} = 2^3 = 8$ subsets:

$$\mathcal{P}(S) = \left\{\varnothing,\ \{1\},\ \{2\},\ \{3\},\ \{1,2\},\ \{1,3\},\ \{2,3\},\ \{1,2,3\}\right\}.$$

### Problem 6 — Big-O: $4n^2 + 2 = O(n^2)$

**Solution.** We need constants $c > 0$ and $n_0$ with $4n^2 + 2 \le c n^2$ for all $n \ge n_0$. Take $c = 5$, $n_0 = 2$: since $2 \le n^2$ for all $n \ge 2$,

$$4n^2 + 2 \le 4n^2 + n^2 = 5n^2, \qquad \forall\, n \ge 2.$$

So $4n^2 + 2 = O(n^2)$ with $c = 5$, $n_0 = 2$.

### Problem 7 — Big-Ω: $4n^2 + 2 = \Omega(2n)$

**Solution.** We need $4n^2 + 2 \ge c \cdot 2n$ for all large $n$. Take $c = 1$, $n_0 = 2$: since $4n^2 \ge 2n$ for all $n \ge 1$ (i.e., $2n \ge 1$),

$$4n^2 + 2 \ge 4n^2 \ge 2n = 1 \cdot (2n), \qquad \forall\, n \ge 2.$$

So $4n^2 + 2 = \Omega(2n)$ with $c = 1$, $n_0 = 2$.

### Problem 8 — Big-Ω: $3n + 2$ vs. $n$

Show $f(n) = 3n + 2$ is $\Omega(g(n))$ for $g(n) = n$.

**Solution.** Take $c = 1$, $n_0 = 1$: since $3n + 2 \ge n$ for all $n \ge 1$, we have $f(n) = \Omega(n)$. (In fact it is tighter: with $c_1 = 1$, $c_2 = 4$, $n_0 = 2$ — since $3n + 2 \le 4n$ for all $n \ge 2$ — we get $f(n) = \Theta(n)$.)

### Problem 9 — Showing something is *not* Big-O: $n^3 + n^2 \ne O(n^2)$

**Solution (by contradiction).** Suppose $n^3 + n^2 = O(n^2)$. Then for some $c > 0$ and all $n \ge n_0$:

$$n^3 + n^2 \le c\, n^2.$$

Dividing by $n^2 > 0$ gives $n + 1 \le c$ for **all** $n \ge n_0$. But $n + 1$ grows without bound — pick any $n \ge \max(n_0,\, c)$ and we get $c < n + 1 \le c$, a contradiction. Hence no such $c$ exists and

$$n^3 + n^2 \ne O(n^2).$$

### Problem 10 — Big-Θ: $n^4 + 2n^2 = \Theta(2n^4)$

**Solution.** Take $c_1 = 0.1$, $c_2 = 10$, $n_0 = 2$. For all $n \ge 2$:

$$0.1 \cdot (2n^4) = 0.2 n^4 \le n^4 + 2n^2 \le n^4 + 2n^4 = 3n^4 \le 10 \cdot (2n^4).$$

So $n^4 + 2n^2 = \Theta(2n^4)$ with $c_1 = 0.1$, $c_2 = 10$, $n_0 = 2$.

### Problem 11 — Growth-rate relations (handout example)

Let $f(n) = 2n^2 + 3n$, $g(n) = n^3$, $h(n) = 10n^2 + 100$. Show:

**(a)** $f(n) = O(g(n))$.
Take $c = 1$, $n_0 = 10$: for all $n \ge 10$, $2n^2 + 3n \le n^3$ (since $2n + 3/n \le n$).

**(b)** $g(n) = \Omega(h(n))$.
Take $c = 1$, $n_0 = 10$: for all $n \ge 10$, $n^3 \ge 10n^2 + 100$ (since $n \ge 10 + 100/n$).

**(c)** $f(n) = \Theta(h(n))$.
Take $c_1 = 0.1$, $c_2 = 5$, $n_0 = 10$: for all $n \ge 10$,

$$0.1\, h(n) = n^2 + 10 \le 2n^2 + 3n \le 5(10n^2 + 100) = 5\, h(n).$$

### Problem 12 — Induction: sum of the first $n$ integers

Prove by induction that for all $n \ge 1$:

$$1 + 2 + 3 + \cdots + n = \frac{n(n+1)}{2}.$$

**Solution.** Let $P(n)$ be the statement above.

*Base case ($n = 1$):* LHS $= 1$, RHS $= \frac{1(1+1)}{2} = 1$. So $P(1)$ is true.

*Inductive step:* Assume $P(k)$ for some $k \ge 1$:

$$1 + 2 + \cdots + k = \frac{k(k+1)}{2}.$$

Add the $(k+1)$-st term to both sides:

$$1 + 2 + \cdots + k + (k+1) = \frac{k(k+1)}{2} + (k+1) = \frac{(k+1)(k + 2)}{2},$$

which is exactly $P(k+1)$ (with $n = k+1$). By induction, the formula holds for all $n \ge 1$. $\blacksquare$

### Problem 13 — Induction: $n^3 - n$ divisible by 3

Prove by induction that for every integer $n \ge 2$, $n^3 - n$ is divisible by 3.

**Solution.** Let $P(n)$ be "$n^3 - n$ is divisible by 3."

*Base case ($n = 2$):* $2^3 - 2 = 8 - 2 = 6 = 3 \cdot 2$. So $P(2)$ is true.

*Inductive step:* Assume $P(k)$: $k^3 - k = 3m$ for some integer $m$. Consider $(k+1)^3 - (k+1)$:

$$(k+1)^3 - (k+1) = k^3 + 3k^2 + 3k + 1 - k - 1 = \underbrace{(k^3 - k)}_{= 3m} + 3k^2 + 3k = 3(m + k^2 + k),$$

which is divisible by 3. So $P(k+1)$ holds, and the claim follows for all $n \ge 2$. $\blacksquare$

### Problem 14 — Induction: sum of the first $n$ odd numbers

Prove by induction that for all $n \ge 1$:

$$\sum_{i=1}^{n} (2i - 1) = n^2.$$

**Solution.** Let $P(n)$ be the statement above.

*Base case ($n = 1$):* $\sum_{i=1}^{1}(2i-1) = 1 = 1^2$. So $P(1)$ is true.

*Inductive step:* Assume $P(k)$: $\sum_{i=1}^{k}(2i - 1) = k^2$. Then

$$\sum_{i=1}^{k+1}(2i-1) = \underbrace{\sum_{i=1}^{k}(2i-1)}_{=\, k^2} + (2(k+1) - 1) = k^2 + 2k + 1 = (k+1)^2,$$

which is $P(k+1)$. By induction the identity holds for all $n \ge 1$. $\blacksquare$

### Problem 15 — Strong induction: every integer $\ge 2$ has a prime divisor

Show that every integer $n \ge 2$ is divisible by a prime number.

**Solution (strong induction).** Let $P(n)$ be "$n$ is divisible by a prime."

*Base case:* $P(2)$ holds since 2 itself is prime.

*Inductive step:* Assume $P(i)$ for **all** integers $2 \le i < k+1$. We prove $P(k+1)$. Two cases:

- **(a)** $k + 1$ is prime — then it is divisible by a prime (itself).
- **(b)** $k + 1$ is composite — then $k+1 = ab$ with $2 \le a, b \le k$. By the strong inductive hypothesis, $a$ is divisible by some prime $p$, and since $p \mid a$ and $a \mid (k+1)$, we get $p \mid (k+1)$.

In either case $P(k+1)$ holds. By strong induction, every integer $\ge 2$ has a prime divisor. $\blacksquare$

### Problem 16 — Contradiction: rational + irrational is irrational

Show that if $x$ is rational and $y$ is irrational, then $x + y$ is irrational.

**Solution.** Assume for contradiction that $x + y$ is rational. Since the difference of two rationals is rational, $(x + y) - x = y$ would be rational — contradicting that $y$ is irrational. Therefore $x + y$ is irrational. $\blacksquare$

### Problem 17 — Contradiction: rational − irrational is irrational

Show that if $x$ is rational and $y$ is irrational, then $x - y$ is irrational (with an explicit fraction argument).

**Solution.** Assume for contradiction that $x - y$ is rational. Write $x - y = \frac{a}{b}$ with integers $a, b$, $b \ne 0$, and write $x = \frac{d}{c}$ with integers $d, c$, $c \ne 0$. Then

$$y = x - (x - y) = \frac{d}{c} - \frac{a}{b} = \frac{bd - ac}{bc},$$

where $bd - ac$ and $bc$ are integers with $bc \ne 0$. So $y$ is rational — a contradiction. Hence $x - y$ is irrational. $\blacksquare$

### Problem 18 — Contradiction: if $a^2$ is even then $a$ is even

Show that for all $a \in \mathbb{Z}$, if $a^2$ is even then $a$ is even.

**Solution.** Assume $a^2$ is even but $a$ is odd. Then $a = 2c + 1$ for some integer $c$, and

$$a^2 = (2c+1)^2 = 4c^2 + 4c + 1 = 2(2c^2 + 2c) + 1,$$

which is odd — contradicting that $a^2$ is even. Therefore $a$ must be even. $\blacksquare$

### Problem 19 — Contradiction: $a^2 - 4b \ne 2$ for all integers $a, b$

Show that for all $a, b \in \mathbb{Z}$, $a^2 - 4b \ne 2$.

**Solution.** Assume for contradiction that $a^2 - 4b = 2$ for some integers $a, b$. Then $a^2 = 2 + 4b$, the sum of two even numbers, so $a^2$ is even. By Problem 18, $a$ is even: write $a = 2c$. Substituting:

$$4c^2 - 4b = 2 \implies 2(c^2 - b) = 1.$$

But the left side is an even integer and the right side is odd — a contradiction. Hence $a^2 - 4b \ne 2$ for all integers $a, b$. $\blacksquare$

---

## Quick Reference

| Symbol | Meaning |
|---|---|
| $x \in S$, $x \notin S$ | membership / non-membership |
| $\varnothing$ or $\{\}$ | empty set |
| $A \subseteq B$, $A \subset B$ | subset / proper subset |
| $A = B$ | iff $A \subseteq B$ and $B \subseteq A$ |
| $A \cup B$, $A \cap B$, $A - B$ | union, intersection, difference |
| $\overline{A}$ | complement (relative to universal set $U$) |
| $\mathcal{P}(S)$ or $2^S$ | powerset; $|\mathcal{P}(S)| = 2^{|S|}$ |
| $A \times B$ | Cartesian product of ordered pairs |
| $f : A \to B$ | function from domain $A$ to codomain $B$ |
| $O(g)$, $\Omega(g)$, $\Theta(g)$ | upper bound / lower bound / tight bound |
