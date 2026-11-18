---
title: "Designing Vortex Codathon: Making Simple Problems Uncomfortable"
date: 2026-01-12
permalink: /posts/2026/01/vortex-codathon/
excerpt: An architectural breakdown of the few problems designed for Vortex 13.0 -- removing a few of the usual conveniences, making simple computations strangely unpleasant, and eventually arriving at process control, game theory, elliptic curves, and particle dynamics.
tags:
  - algorithmic-design
  - scientific-computing
  - dynamical-systems
---

> **Contest Artifacts & Codebase:**  
> * **Source Repository:** [`Vortex-ICT-Mumbai/Vortex_13.0/events/prodigy/codathon`](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon) -- problem statements, scripts, and reference implementations.
> * **Problem Papers:** | [Round 1 PDF]({{ base_path }}/files/Codathon_R1.pdf) | [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)

<!-- MORE BUTTON: Anything below this line only shows when the user clicks the post -->
<!--more-->

---

Most competitive programming contests reward speed-typing and template recall: match a problem to a standard algorithm (Dijkstra, Segment Trees, sliding window), dump boilerplate, and move on. 

When designing the problem sets for **Vortex Codathon** (Round 1 and Round 2), the objective was different. We wanted problems that test whether a solver actually understands the mathematical machinery they routinely outsource to standard libraries. If you strip away `scipy.optimize`, `numpy.linalg`, floating-point smoothers, and pre-baked abstractions, what remains is the raw structure of the problem; most of what we call basic is _basic_ only because somebody else already did the work.

---

### 1. Stripping the Abstraction: Imperative vs. Declarative Knowledge

The Intel 8085's arithmetic stops at add, subtract, increment, decrement and a decimal adjust. It has no multiply, no divide, no square root and no floating point at all. Multiplication is a subroutine you write yourself: repeated addition, or the faster shift-and-add, doubling a register pair with DAD H and adding wherever the multiplier has a 1 bit.

  * Compute \\(\sqrt{x}\\) using only basic arithmetic and loops (no `**`, `pow()`, or math libraries), with the condition that the method must converge
  * Implement \\(\text{abs}(x)\\) and \\(\max(a, b)\\) without `<`, `>`, `if`, or libraries.
  * Compute division \\(a / b\\) using only `+`, `-`, and `*` without `/`, `//`, `%`, or branching.  

*The Mechanism:* Confronts the difference between imperative execution (bit-level sign extraction, fixed-point subtraction, Newton-Raphson iterations) and declarative trust.

