---
title: "Designing Vortex Codathon: Embedded Physics & Algorithmic Traps"
date: 2026-01-12
permalink: /posts/2026/01/vortex-codathon/
tags:
  - algorithmic-design
  - scientific-computing
  - dynamical-systems
---

[GitHub Repository](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon) • [Round 1 PDF]({{ base_path }}/files/Codathon_R1.pdf) • [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)
An architectural breakdown of the problem sets designed for Vortex 13.0, testing systems intuition, non-ideal particle dynamics, and numerical constraints without library abstractions.

> **Contest Artifacts & Codebase:**  
> * **Source Repository:** [`Vortex-ICT-Mumbai/Vortex_13.0/events/prodigy/codathon`](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon) -- problem statements, automated evaluators, simulation scripts, and reference implementations.
> * **Problem Papers:** [Round 1 PDF]({{ base_path }}/files/Codathon_R1.pdf) | [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)

<!-- MORE BUTTON: Anything below this line only shows when the user clicks the post -->
<!--more-->

---

Most competitive programming contests reward speed-typing and template recall: match a problem to a standard algorithm (Dijkstra, Segment Trees, sliding window), dump boilerplate, and move on. 

When designing the problem sets for **Vortex Codathon** (Round 1 and Round 2), the objective was different. We wanted problems that test whether a solver actually understands the mathematical machinery they routinely outsource to standard libraries. If you strip away `scipy.optimize`, `numpy.linalg`, floating-point smoothers, and pre-baked abstractions, what remains is the raw structure of the problem.

Here is an architectural walkthrough of the mathematical intuition, structural constraints, and dynamical concepts embedded into these papers.

---

### 1. Stripping the Abstraction: Imperative vs. Declarative Knowledge

In modern engineering workflows, calling an optimization routine or computing a definite integral takes a single line of code. But declarative abstractions conceal the mechanical suffering required to build them.
