---
title: "Designing Vortex Codathon: Embedded Physics & Algorithmic Traps"
date: 2026-01-12
permalink: /posts/2026/01/vortex-codathon/
tags:
  - algorithmic-design
  - scientific-computing
  - dynamical-systems
---

An architectural breakdown of the problem sets designed for Vortex 13.0, testing systems intuition, non-ideal particle dynamics, and numerical constraints without library abstractions.

> **Contest Artifacts & Codebase:**  
> * **Source Repository:** [`Vortex-ICT-Mumbai/Vortex_13.0/events/prodigy/codathon`](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon) -- problem statements, automated evaluators, simulation scripts, and reference implementations.
> * **Problem Papers:** | [Round 1 PDF]({{ base_path }}/files/Codathon_R1.pdf) | [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)

<!-- MORE BUTTON: Anything below this line only shows when the user clicks the post -->
<!--more-->

---

Most competitive programming contests reward speed-typing and template recall: match a problem to a standard algorithm (Dijkstra, Segment Trees, sliding window), dump boilerplate, and move on. 

When designing the problem sets for **Vortex Codathon** (Round 1 and Round 2), the objective was different. We wanted problems that test whether a solver actually understands the mathematical machinery they routinely outsource to standard libraries. If you strip away `scipy.optimize`, `numpy.linalg`, floating-point smoothers, and pre-baked abstractions, what remains is the raw structure of the problem.

Here is an architectural walkthrough of the mathematical intuition, structural constraints, and dynamical concepts embedded into these papers.

---

### 1. Stripping the Abstraction: Imperative vs. Declarative Knowledge

In modern engineering workflows, calling an optimization routine or computing a definite integral takes a single line of code. But declarative abstractions conceal the mechanical suffering required to build them.

* **The 8085 Constraint (R1, Q1):** Solvers were stripped of high-level features and tasked with computing basic operations on primitive terms:
  * Compute \\(\sqrt{x}\\) using only basic arithmetic and loops (no `**`, `pow()`, or math libraries).
  * Implement \\(\text{abs}(x)\\) and $\max(a, b)\\) without `<`, `>`, `if`, or libraries.
  * Compute division \\(a / b\\) using only `+`, `-`, and `*` without `/`, `//`, `%`, or branching.  
  * *The Mechanism:* Confronts the difference between imperative execution (bit-level sign extraction, fixed-point subtraction, Newton-Raphson iterations) and declarative trust.
* **Expensive Evaluations (R1, Q9):** Solvers were handed an Equal-Interval Search Method (EISM) locating a 1D minimum over \\([a, b]\\) and told to minimize runtime:
  * *The Trap:* Attempting code-level micro-optimizations.
  * *The Mechanism:* The actual bottleneck in black-box optimization is evaluating \\(f(x)\\). By transitioning from fixed trisection to Golden Section search, one of the interior evaluation points is reused across successive iterations (\\(\tau = \frac{\sqrt{5}-1}{2} \approx 0.618\\)), reducing costly evaluations by nearly half without sacrificing convergence tolerance.

---

### 2. Secrecy by Structure: Elliptic Curves Over $\mathbb{F}_{23}$ (R2, Q2)

Set in an Orwellian regime where floating-point drift and continuity are outlawed, "Big Brother & The Finite Curve" forces solvers to compute cryptographic handshakes on a discrete grid:

$$\mathcal{E}: y^2 \equiv x^3 - 19x + 84 \pmod{23} \quad$$
