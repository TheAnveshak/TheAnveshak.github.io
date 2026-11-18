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

## 1. Stripping the Abstraction: Imperative vs. Declarative Knowledge

The Intel 8085's arithmetic stops at add, subtract, increment, decrement and a decimal adjust. It has no multiply, no divide, no square root and no floating point at all. Multiplication is a subroutine you write yourself: repeated addition, or the faster shift-and-add, doubling a register pair with `DAD H` and adding wherever the multiplier has a 1 bit.

* Compute \\(\sqrt{x}\\) using only basic arithmetic and loops (no `**`, `pow()`, or math libraries), with the condition that the method must converge.
* Implement \\(\text{abs}(x)\\) and \\(\max(a, b)\\) without `<`, `>`, `if`, or libraries.
* Compute division \\(a / b\\) using only `+`, `-`, and `*` without `/`, `//`, `%`, or branching.

*The Mechanism:* Confronts the difference between imperative execution (bit-level sign extraction, fixed-point subtraction, Newton-Raphson iterations) and declarative trust.

The four sub-questions look independent. They are just one question asked four times, and the 2.5\\(\times\\) multiplier was there to see who noticed.

### 1.1 One Hammer

Give someone Newton–Raphson and every problem starts to look like a root. That is usually a warning. Here it was the intended path.

\\(\sqrt{a}\\) is the root of \\(f(x) = x^2 - a\\), and one Newton step collapses to an average:

$$x_{n+1} = x_n - \frac{x_n^2 - a}{2x_n} = \frac{1}{2}\left(x_n + \frac{a}{x_n}\right).$$

The same rule falls out of a single observation. The product \\(x \cdot a/x\\) is always \\(a\\), so if \\(x\\) overshoots \\(\sqrt{a}\\), then \\(a/x\\) undershoots it by the same factor. The root is trapped between the pair, and their average is the obvious next guess. By AM-GM that average never falls below \\(\sqrt{a}\\), so from the first step on the sequence is bounded below and non-increasing, and a monotone bounded sequence converges. How fast it converges is a single identity:

$$x_{n+1} - \sqrt{a} = \frac{(x_n - \sqrt{a})^2}{2x_n}.$$

The error squares each step, from any \\(x_0 > 0\\). That settles *"your method must converge"*. Everything else in Q1 is built on it.

```python
def sqrt(a, x=1.0, n=60):
    for _ in range(n):
        x = 0.5*(x + a/x)
    return x
```
One concession, made quietly: a loop is not branch-free either. Compiled, `for i in range(n)` becomes an increment, a compare and a conditional jump (`addl $1`, `cmpl`, `jne` on x86-64; `DCR C`, `JNZ` on the 8085), and Python's `for` keeps the same test inside `range`'s iterator. Loops were allowed anyway. Without them nothing here can be written at all.

### 1.2 Then Everything Is a Nail

Once you have a square root, the rest needs no comparisons at all:

$$\lvert x \rvert = \sqrt{x^2}, \qquad \max(a,b) = \frac{a+b}{2} + \frac{\lvert a-b \rvert}{2}, \qquad \min(a,b) = \frac{a+b}{2} - \frac{\lvert a-b \rvert}{2}.$$

That is the midpoint, plus or minus half the gap. The `if` hasn't disappeared. It has moved into the initial guess: Newton started from \\(x_0 > 0\\) can only land on the positive root.

```python
def absolute(x):
    return sqrt(x*x)

def maximum(a, b):
    return 0.5*(a + b + absolute(a - b))

def minimum(a, b):
    return 0.5*(a + b - absolute(a - b))
```

These carry the assumptions the section title warns about. \\(x^2\\) overflows to `inf` once \\(\lvert x \rvert \gtrsim 10^{154}\\), and the halving is written `* 0.5` because `/` was banned.

### 1.3 The Nail That Fights Back: Division

The obvious residual for \\(1/b\\) is \\(1 - bx\\). Newton solves a linear function in one step, but that step divides by \\(b\\): division built out of division. The residual is yours to choose, so choose \\(f(x) = 1/x - b\\):

$$x_{n+1} = x_n - \frac{1/x_n - b}{-1/x_n^2} = x_n\,(2 - b\,x_n).$$

The reciprocals cancel and only `*` and `-` remain. With \\(e_n = 1 - b\,x_n\\),

$$e_{n+1} = e_n^2.$$

So it converges quadratically when \\(0 < x_0 < 2/b\\) and goes to \\(-\infty\\) otherwise. The whole problem is the initial guess. Hardware scales \\(b\\) into \\([0.5, 1]\\) by powers of two and starts at 1. Without branches, \\(x_0 = \varepsilon b\\) works for every \\(\lvert b \rvert < \sqrt{\frac{2}{\varepsilon}}\\) and carries the sign for free, at the cost of a slow doubling phase (about 97 steps for \\(b = 15.2\\) with \\(\varepsilon = 10^{-30}\\)).

```python
def reciprocal(b, eps=1e-30, n=200):
    x = eps*b
    for _ in range(n):
        x = x*(2.0 - b*x)
    return x

def divide(a, b):
    return a*reciprocal(b)
```

This did not end with the 8085. In higher dimensions there is no `/` to fall back on at all, and \\(X_{k+1} = X_k(2I - AX_k)\\) (Newton–Schulz) inverts a matrix whenever \\(\rho(I - AX_0) < 1\\). It is the same residual, squared.

---

## 2. Scientific Computing, Unwrapped (Q2)

Q2 asked for any one of three operations that we usually get as a single library call: a definite integral, a gradient from sampled points, and a model fitted to data, the "real" question: *"All design choices are left to you and will be evaluated."*

### 2.1 The Integral

The most common submission was a left Riemann sum in a `while` loop:

```python
x = a
while x <= b:
    area += f(x)*dx
    x += dx
```

It works, and it hides two decisions. The first is accuracy, a Riemann sum has error \\(O(h)\\); the trapezoid rule uses the same function evaluations and gets \\(O(h^2)\\); Simpson's rule gets \\(O(h^4)\\). The second is floating point. `0.1` is not representable in binary, so `x += 0.1` drifts: after ten steps `x` is `0.9999999999999999`, the `<=` admits one more point, and the loop runs eleven times instead of ten. Indexing with integers fixes both:

```python
def trapz(f, a, b, N=1000):
    h = (b - a)/N
    s = 0.5*(f(a) + f(b))
    for i in range(1, N):
        s += f(a + i*h)
    return s*h
```

### 2.2 The Gradient

The input was an array of samples, not a function, so the spacing \\(h\\) is given rather than chosen. Central differences are second order in the interior; the edges need one-sided stencils. If the samples are not evenly spaced, the three-point formula has to account for that:

```python
def gradient(x, y):
    n = len(x)
    g = [0.0]*n
    g[0] = (y[1] - y[0])/(x[1] - x[0])
    g[-1] = (y[-1] - y[-2])/(x[-1] - x[-2])
    for i in range(1, n-1):
        hl, hr = x[i] - x[i-1], x[i+1] - x[i]
        g[i] = (hl*hl*y[i+1] - hr*hr*y[i-1] + (hr*hr - hl*hl)*y[i])/(hl*hr*(hl + hr))
    return g
```

The deeper point is an asymmetry. Integration averages, so noise in the samples largely cancels. Differentiation divides by \\(h\\), so noise of size \\(\delta\\) comes out as roughly \\(\delta/h\\). When you can choose \\(h\\) (with a function handle rather than data), truncation error falls like \\(h^2\\) while round-off grows like \\(\epsilon/h\\), and the best \\(h\\) sits near \\(\epsilon^{1/3} \approx 6\times10^{-6}\\) for doubles. With data you cannot choose, so the real design decision is whether to smooth before you differentiate. Integration forgives; differentiation is ill-posed.

### 2.3 The Fit

A straight line by least squares has a closed form that needs nothing beyond sums:

```python
def fit_line(x, y):
    n = len(x)
    Sx, Sy = sum(x), sum(y)
    Sxx = sum(xi*xi for xi in x)
    Sxy = sum(xi*yi for xi, yi in zip(x, y))
    m = (n*Sxy - Sx*Sy)/(n*Sxx - Sx*Sx)
    c = (Sy - m*Sx)/n
    return m, c
```

The design choice here is the error measure, and it is a modelling assumption rather than a convention. Squared error is the maximum-likelihood choice under Gaussian noise; absolute error corresponds to heavier-tailed noise and is far less bothered by outliers. Anyone who instead minimised the loss by gradient descent ran into a familiar condition: on a quadratic loss with Hessian eigenvalues \\(\lambda\\), the step size \\(\eta\\) converges only if \\(\lvert 1 - \eta\lambda \rvert < 1\\) for all of them.
