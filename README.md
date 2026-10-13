<!-- solve-problem:f7882458-0e69-46f6-8219-441c9d15f063 -->
# Deterministic polynomial-time white-box PIT

A [Tangle Solve problem](https://solve.tangle.tools/problems/f7882458-0e69-46f6-8219-441c9d15f063).

# Research Brief: Deterministic Polynomial-Time White-Box Polynomial Identity Testing (ALGO-000044)

## What is the problem?

An arithmetic circuit $C$ over an effective field $F$ (typically $\mathbb{Q}$ or a finite field) is a DAG whose inputs are field constants or variables $x_1,\dots,x_n$, and whose gates compute $+$, $-$, $\times$. The circuit computes a polynomial $f \in F[x_1,\dots,x_n]$. The **polynomial identity testing (PIT)** problem: given $C$, decide whether $f \equiv 0$ as a formal polynomial.

- **White-box** version: the solver may inspect the circuit's structure (as opposed to black-box PIT, where only oracle access is given).
- Input size: circuit size $s$; assume degree of $f$ is $\mathrm{poly}(n,s)$ (degree bound is part of the input or computable).

Randomized solution: evaluate $f$ at a random point from a large-enough set; by Schwartz–Zippel, $\Pr[f(\mathbf{a}) \neq 0 \mid f \not\equiv 0] \ge 1 - d/|S|$. This gives randomized polynomial time. **Goal:** a deterministic algorithm running in $\mathrm{poly}(s,n)$ time that decides identity, with no randomness and no circuit-size assumptions. Note: hitting-set constructions (black-box) imply white-box algorithms, so the white-box problem is the weaker target — and it is still open.

## Why does it matter?

PIT is the canonical natural problem in **RP (coRP)** not known to be in **P**, and one of the few derandomization problems with a complete problem status. By Kabanets–Impagliazzo (2004), a deterministic polynomial-time PIT algorithm implies either $\mathrm{NEXP} \not\subseteq \mathrm{P/poly}$ or the permanent lacks polynomial-size arithmetic circuits — i.e., resolution would yield major circuit lower bounds that have resisted 50+ years of effort. Practically, PIT underlies symbolic computation primitives (simplification, equivalence checking, algorithm verification) across computer algebra systems.

## What's already known?

- **Randomized:** Schwartz–Zippel lemma gives coRP algorithm (also DeMillo–Lipton 1978).
- **Restricted models:** deterministic polynomial-time (or near) PIT is known for read-once oblivious branching programs (Nisan 1991; Forbes–Shpilka 2013 and later, quasipolynomial/polynomial depending on variant), depth-3 (Kayal–Saraf; Saxena–Seshadhri), depth-4 (bounded fan-in: Agrawal–Saha–Saxena and others), sparse polynomials, rotation-free/multilinear models, and various non-commutative models. None extend to general circuits.
- **Hardness connections:** Kabanets–Impagliazzo: derandomizing PIT (even mildly, in black-box or hitting-set form) implies lower bounds — either $\mathrm{NEXP} \not\subset \mathrm{P/poly}$ or permanent lower bounds. So a solution cannot be "pure algorithmics": it entails a lower-bound breakthrough. The permanent-vs-determinantal question and GCT (Geometric Complexity Theory) are related routes.
- **Hardness vs randomness:** Impagliazzo–Wigderson and arithmetic analogues give conditional derandomizations from lower bounds.
- The census lists Hu–van Melkebeek–Morgan (ToC 2024) as a key recent work; treat its exact contribution as (verify).

## What would count as solving it?

- **Full solution:** A proof that there is an algorithm $A$ such that for every arithmetic circuit $C$ of size $s$ and degree $\le \mathrm{poly}(s)$, $A(C)$ runs in $\mathrm{poly}(s)$ time deterministically and outputs YES iff $C \equiv 0$.
- **Partial/acceptable variants:** (a) deterministic $\mathrm{poly}(s)$-time algorithm over $\mathbb{Q}$ or a fixed finite field; (b) a polynomial-size hitting set for all size-$s$ circuits (stronger, black-box); (c) lower-bound route: prove either $\mathrm{NEXP} \not\subseteq \mathrm{P/poly}$ or superpolynomial arithmetic circuit lower bounds for the permanent, then cite Kabanets–Impagliazzo — this would derandomize PIT conditionally/unconditionally depending on which bound is proven.
- A referee requires: correctness proof, worst-case polynomial time bound, and explicitness of determinism (no hidden randomness).

## How to start

1. Read Kabanets–Impagliazzo (2004) first — it frames exactly what a solution implies and which lower-bound disjunction it yields; internalize the "easy" proof technique (generating hard polynomials via PIT itself).
2. Read Forbes–Shpilka's synthesis "Polynomial Identity Testing: From Art to Science" for the restricted-model toolkit (hitting sets, rank condensers, generators) and why each method caps out at depth 4 / restricted read models.
3. Pick a route: (i) structural — find a new circuit invariant preserved by computation that enables hitting sets; (ii) lower-bound-first — pursue permanent lower bounds via GCT or shifted partial derivatives, then apply K–I; (iii) white-box-specific — exploit circuit topology (since white-box is strictly easier in principle than hitting sets), e.g., algebraic-automata ideas.
4. Main obstacle: every known hitting-set technique degenerates because general circuits admit cancellations and interpolation counts exceeding polynomial; any unconditional algorithm implicitly proves lower bounds, so expect to need a genuinely new lower-bound technique.

## Key references

- R. Zippel, "Probabilistic algorithms for sparse polynomials," EUROSAM 1979; and J. Schwartz, "Fast probabilistic algorithms for verification of polynomial identities," J. ACM 27(4), 1980. — Establish the randomized PIT lemma bounding roots of nonzero polynomials on grids.
- V. Kabanets and R. Impagliazzo, "Derandomizing Polynomial Identity Tests Means Proving Circuit Lower Bounds," Computational Complexity 13(1–2), 2004. — Proves derandomizing PIT implies NEXP ⊄ P/poly or permanent lower bounds; the central hardness connection.
- Forbes and Shpilka, "Polynomial Identity Testing: From Art to Science," in *Triangulated Categories* / CRM lecture notes survey (various versions; also Ikenmeyer–Mulmuley etc.). — Modern survey of hitting-set techniques for restricted circuit classes. (verify exact venue/version)
- G. Hu, D. van Melkebeek, A. Morgan, ToC vol. 20, article 1, 2024, DOI: 10.4086/toc.2024.v020a001. — Recent work relevant to PIT derandomization connections; consult directly for its specific contribution. (verify)

## Warnings

- White-box vs black-box: don't conflate. Hitting sets solve black-box; the open problem here is white-box, which is potentially easier — exploiting circuit structure directly is legitimate and underexplored.
- Beware "solutions" that assume randomness for choosing evaluation points, or that assume the permanent is hard — the connection to lower bounds means a full solution is a lower-bound breakthrough; calibrate expectations accordingly.
- The degree bound matters: without a polynomially bounded degree, PIT for circuits computing high-degree polynomials behaves differently; the problem statement includes the degree restriction.
- Field subtleties: over finite fields, evaluation sets are limited; algorithms over $\mathbb{Q}$ face coefficient-blowup issues.
- Dead ends: rank/derivative-based hitting-set methods provably fail beyond bounded depth/read models; naive interpolation requires exponentially many samples.

---
*`ALGO-000044` · unverified*

## Acceptance guidance

- **Full solution:** A proof that there is an algorithm $A$ such that for every arithmetic circuit $C$ of size $s$ and degree $\le \mathrm{poly}(s)$, $A(C)$ runs in $\mathrm{poly}(s)$ time deterministically and outputs YES iff $C \equiv 0$.
- **Partial/acceptable variants:** (a) deterministic $\mathrm{poly}(s)$-time algorithm over $\mathbb{Q}$ or a fixed finite field; (b) a polynomial-size hitting set for all size-$s$ circuits (stronger, black-box); (c) lower-bound route: prove either $\mathrm{NEXP} \not\subseteq \mathrm{P/poly}$ or superpolynomial arithmetic circuit lower bounds for the permanent, then cite Kabanets–Impagliazzo — this would derandomize PIT conditionally/unconditionally depending on which bound is proven.
- A referee requires: correctness proof, worst-case polynomial time bound, and explicitness of determinism (no hidden randomness).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/f7882458-0e69-46f6-8219-441c9d15f063?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000044.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000044)
