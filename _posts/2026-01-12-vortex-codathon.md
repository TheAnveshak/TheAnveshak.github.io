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
> * **Source Repository:** [`Vortex-ICT-Mumbai/Vortex_13.0/events/prodigy/codathon`](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon) -- problem statements, scripts, and reference implementations.
> * **Problem Papers:** | [Round 1 PDF]({{ base_path }}/files/Codathon_R1.pdf) | [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)

<!-- MORE BUTTON: Anything below this line only shows when the user clicks the post -->
<!--more-->

---

Most competitive programming contests reward speed-typing and template recall: match a problem to a standard algorithm (Dijkstra, Segment Trees, sliding window), dump boilerplate, and move on. 

When designing the problem sets for **Vortex Codathon** (Round 1 and Round 2), the objective was different. We wanted problems that test whether a solver actually understands the mathematical machinery they routinely outsource to standard libraries. If you strip away `scipy.optimize`, `numpy.linalg`, floating-point smoothers, and pre-baked abstractions, what remains is the raw structure of the problem.

---

### 1. Stripping the Abstraction: Imperative vs. Declarative Knowledge

In modern engineering workflows, calling an optimization routine or computing a definite integral takes a single line of code. But declarative abstractions conceal the mechanical suffering required to build them.

* **The 8085 Constraint (R1, Q1):** Solvers were stripped of high-level features and tasked with computing basic operations on primitive terms:
  * Compute \\(\sqrt{x}\\) using only basic arithmetic and loops (no `**`, `pow()`, or math libraries).
  * Implement \\(\text{abs}(x)\\) and \\(\max(a, b)\\) without `<`, `>`, `if`, or libraries.
  * Compute division \\(a / b\\) using only `+`, `-`, and `*` without `/`, `//`, `%`, or branching.  
  * *The Mechanism:* Confronts the difference between imperative execution (bit-level sign extraction, fixed-point subtraction, Newton-Raphson iterations) and declarative trust.

---

### 2. Secrecy by Structure: Elliptic Curves Over \\(\mathbb{F}_{23}\\) (R2, Q2)

Set in an Orwellian regime where floating-point drift and continuity are outlawed, "Big Brother & The Finite Curve" forces solvers to compute cryptographic handshakes on a discrete grid:

$$\mathcal{E}: y^2 \equiv x^3 - 19x + 84 \pmod{23} \quad$$

* **The Problem:** Winston and Julia coordinate meeting locations by adding points \\(P_W\\) and \\(P_J\\) using the curve group law to yield \\(P_M\\). Solvers had to:
  1. Determine valid coordinate pairs given the Oracle's output point \\((16, 9\\).
  2. Compute \\(P_M = P_W + P_J\\) directly when Winston is at \\((3, 10)\\) and Julia is at \\((18, 13)\\).
* **The Mechanism:** On the real plane \\(\mathbb{R}^2\\), point addition draws a line through \\(P\\) and \\(Q\\), finds the third root on the cubic curve, and reflects across the \\(x\\)-axis. Over \\(\mathbb{F}_{23}\\), continuous lines become modular congruences:

$$m = \frac{y_2 - y_1}{x_2 - x_1} \pmod{23} = (y_2 - y_1)(x_2 - x_1)^{p-2} \pmod{23}$$

$$x_3 = m^2 - x_1 - x_2 \pmod{23}, \quad y_3 = m(x_1 - x_3) - y_1 \pmod{23}$$

At \\(p = 23\\), Big Brother can exhaustively tabulate the entire state space. But the structural principle remains: forward point multiplication is fast, while the inverse (the Discrete Logarithm Problem) becomes intractable when scaled to cryptographic primes.

---
