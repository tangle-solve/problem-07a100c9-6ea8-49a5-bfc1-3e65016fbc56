<!-- solve-problem:07a100c9-6ea8-49a5-bfc1-3e65016fbc56 -->
# Constant additive error for Bin Packing

A [Tangle Solve problem](https://solve.tangle.tools/problems/07a100c9-6ea8-49a5-bfc1-3e65016fbc56).

## What is the problem?

In one-dimensional Bin Packing, the input is a multiset of item sizes $a_1, \dots, a_n \in (0, 1]$ (rational, given with polynomial bit-length); the goal is to partition items into a minimum number of "bins," each holding items of total size $\le 1$. Let $\mathrm{OPT}(I)$ be this minimum.

**Open problem:** Find a polynomial-time algorithm that, on every instance $I$, outputs a packing using at most $\mathrm{OPT}(I) + C$ bins, where $C$ is a universal constant independent of $n$ and the instance.

This is distinct from the asymptotic PTAS question: an APTAS (de la Vega–Lueker) gives $\mathrm{OPT}(1+\varepsilon) + 1$, whose additive excess grows with $\mathrm{OPT}$; here the excess must be $O(1)$ absolutely. Note the trivial lower bound: exact Bin Packing is strongly NP-hard, so $C \ge 1$ in general and no algorithm can achieve $\mathrm{OPT} + o(1)$; the question is whether some finite constant is achievable in polynomial time. The natural formalization asks for deterministic or randomized polynomial time with constant additive error (state which; both variants are open).

## Why does it matter?

Bin Packing is the canonical packing/scheduling problem; an $O(1)$ additive guarantee would essentially close the gap between efficiently computable and optimal packings for all large instances, directly improving container/truck utilization in logistics and cutting/memory allocation. It is also the flagship test case for the powerful "rounding + discrepancy" methodology: resolving it likely requires new techniques for rounding fractional Gilmore–Gomory solutions with zero (or constant) excess, which would transfer to related problems (scheduling, CSP, configuration LPs). Meanwhile, the optimal additive constant relates to deep questions about the Gilmore–Gomory configuration LP's additive integrality gap.

## What's already known?

- **Fernandez de la Vega & Lueker (1981):** APTAS — $\mathrm{OPT}(1+\varepsilon)+O(1)$ in time polynomial for fixed $\varepsilon$; additive error grows linearly in $\mathrm{OPT}$.
- **Karmarkar & Karp (1982):** Polynomial algorithm using at most $\mathrm{OPT} + O(\log^2 n)$ bins, via the Gilmore–Gomory configuration LP with lazy column generation and clever item rounding. For ~35 years the best bound.
- **Hoberg & Rothvoß (2017):** Improved to $\mathrm{OPT} + O(\log \mathrm{OPT})$ bins, via a refined rounding of the configuration LP using a discrepancy-theoretic walk on the bin-index tree (partial coloring / entropy method). This is the current best and the natural starting point.
- **Eisenbrand–Pálvölgyi–Rothvoß (2011) / other discrepancy work:** the configuration LP can be "rounded" in the sense of grouping items so that the total discrepancy is small; the $\log$ barrier persists because rounding error accumulates over a recursion of depth $\Theta(\log n)$ when item sizes span geometrically many scales. In fact, for the *pure discrepancy* version (rounding each item's fractional assignment to an integral bin while each bin sees discrepancy $O(1)$) a constant is achievable via the Beck–Fiala / six-standard-deviations type results in certain regimes; the obstacle is the interaction of huge item counts with tiny items.
- Known lower-bound/structural results: the configuration LP has integrality gap $1 + O(1/\mathrm{OPT})$, so it is not the obstruction; the loss is in rounding.

## What would count as solving it?

1. **Main goal:** A proof that there is a polynomial-time algorithm and an absolute constant $C$ such that every instance admits an algorithmically-found packing with $\le \mathrm{OPT} + C$ bins. (Explicit $C$ fine; any constant counts.)
2. **Weaker but valuable:** improving $O(\log \mathrm{OPT})$ to $O(\log \log \mathrm{OPT})$, $O(1)$ for restricted item sizes (e.g., bounded distinct sizes, or sizes $\ge 1/\mathrm{polylog}\, n$), or constant additive error with $n^{O(\log n)}$ time.
3. **Lower bound direction:** evidence that constant additive error requires superpolynomial time (e.g., a reduction from an exact-solvability-hard problem, or an integrality/rounding lower bound for a broad class of configuration-LP-based algorithms) would also be a major contribution.

## How to start

1. Read Hoberg & Rothvoß (2017) thoroughly — especially the tree-walking rounding argument — and identify precisely where the $\log$ factor arises (roughly: recursive geometric grouping into $O(\log \mathrm{OPT})$ levels, each contributing constant rounding error).
2. Read Karmarkar–Karp (1982) for the LP/column-generation framework; understand why their geometric-series-of-rounding-thresholds argument inherently loses $\Theta(\log n)$.
3. Natural approach: treat the rounding of small items across scales as a discrepancy problem in a vector space of dimension $\Theta(\log \text{scales})$ and try to beat the trivial bound via a partial-coloring argument with a non-uniform target (bins tolerate slack proportional to their free space; this "slack" structure is not fully exploited in Hoberg–Rothvoß).
4. Main obstacle: tiny items (sizes $\ll 1/n$ or spanning geometric scales) force either recursion or enumeration over exponentially many configurations; the main technical challenge is making rounding error *scale-free*.

## Key references

- M. R. Garey, R. L. Graham, D. S. Johnson, A. C.-C. Yao. "Resource constrained scheduling as generalized bin packing." *J. Combin. Theory B* 21 (1976). Early hardness/structure for bin packing variants. (verify relevance)
- W. Fernandez de la Vega, G. S. Lueker. "Bin packing can be solved within $1+\varepsilon$ in linear time." *Combinatorica* 1 (1981): 349–355. The APTAS via linear-size rounding of item sizes.
- N. Karmarkar, R. M. Karp. "An efficient approximation scheme for the one-dimensional bin-packing problem." *FOCS 1982*. Configuration LP + column generation + iterated rounding giving $\mathrm{OPT} + O(\log^2 n)$.
- R. Hoberg, T. Rothvoß. "A logarithmic additive integrality gap for bin packing." *SODA 2017* / *SIAM J. Discrete Math.* Improves to $\mathrm{OPT} + O(\log \mathrm{OPT})$ via discrepancy-based rounding of the configuration LP on a tree of bins.
- F. Eisenbrand, D. Pálvölgyi, T. Rothvoß. "Bin packing with discrete item sizes." *LATIN 2012* (verify). Polynomial-time exact/O(1)-additive results for instances with few distinct item sizes — a key special case.

## Warnings

- Do not conflate this with the APTAS question — that is long solved and does not give constant additive error.
- Hardness of exact optimization only rules out $C = 0$ (indeed $C \ge 1$); it does not obstruct any positive constant — beware of "obvious impossibility" arguments.
- The phrase "integrality gap" in Hoberg–Rothvoß refers to the configuration LP's additive gap; even a constructive constant additive gap proof there is essentially the whole problem.
- Small items are where all known approaches bleed; don't spend effort on instances with few distinct sizes or all items $\ge \varepsilon$ — those cases already have $\mathrm{OPT}+O(1)$ solutions.
- The census record's attribution of the $O(\log^2 n)$ and $O(\log \mathrm{OPT})$ bounds is correct; anything claiming a constant additive algorithm should be treated with skepticism absent a published proof.

---
*`ALGO-000118` · flagged for review*

## Acceptance guidance

1. **Main goal:** A proof that there is a polynomial-time algorithm and an absolute constant $C$ such that every instance admits an algorithmically-found packing with $\le \mathrm{OPT} + C$ bins. (Explicit $C$ fine; any constant counts.)
2. **Weaker but valuable:** improving $O(\log \mathrm{OPT})$ to $O(\log \log \mathrm{OPT})$, $O(1)$ for restricted item sizes (e.g., bounded distinct sizes, or sizes $\ge 1/\mathrm{polylog}\, n$), or constant additive error with $n^{O(\log n)}$ time.
3. **Lower bound direction:** evidence that constant additive error requires superpolynomial time (e.g., a reduction from an exact-solvability-hard problem, or an integrality/rounding lower bound for a broad class of configuration-LP-based algorithms) would also be a major contribution.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/07a100c9-6ea8-49a5-bfc1-3e65016fbc56?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000118.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000118)
