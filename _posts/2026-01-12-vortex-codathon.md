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

We tend to assume the physical world is continuous-that numbers stretch indefinitely, signals drift smoothly, and spaces yield to continuous intuition. But real processors truncate, sensors round, and computation on bare metal is stubbornly discrete. 

In **"Big Brother & The Finite Curve,"** Winston and Julia abandon continuous channels and encode coordinates onto a discrete group lattice defined by:

$$\mathcal{E}: y^2 \equiv x^3 - 19x + 84 \pmod{23}$$

They combine their private locations \\(P_W\\) and \\(P_J\\) via the curve’s chord-and-tangent group law to yield a meeting coordinate \\(P_M = P_W + P_J\\). 

The mathematical point was never the small-scale arithmetic, but the closure of the space:
* Information from two separate parties merges into a single object.
* Every operation stays strictly inside the finite set--nothing leaks outside.
* Knowing \\(P_M\\) and her own point allows Julia to deduce Winston's location instantly, while an observer seeing only \\(P_M\\) is left with an unresolved combination.

At \\(p = 23\\), the world is small enough for Big Brother to tabulate and exhaust every coordinate. But swap \\(p = 23\\) for a 256-bit prime, and the exact same toy becomes a shield. Forward moves remain trivial; inverting them without a private coordinate becomes computationally impossible. 

This is the intuition behind modern public-key cryptography: not secrecy by hiding, but **secrecy by mathematical structure**--a closed world, a clean group law, and an intractable search space.

---
