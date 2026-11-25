---
title: "CODATHON R2: Finite Is Not Small"
date: 2026-01-12
permalink: /posts/2026/01/vortex-codathon/
excerpt: Notes on Round 2 of Vortex Codathon 13.0 -- point addition on an elliptic curve over a finite prime field, a prisoner's dilemma tournament with noise, and an equilibrium problem.
tags:
  - cryptography
  - scientific-computing
  - dynamical-systems
---

> **Contest Artifacts & Codebase:**  
> * **Source Repository:** [`Vortex-ICT-Mumbai/Vortex_13.0/events/prodigy/codathon`](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon) -- problem statements, scripts, and reference implementations.
> * **Problem Papers:** | [Round 1 PDF]({{ base_path }}/files/Codathon_R1.pdf) | [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)

---

Round 1 banned the libraries. Round 2 allowed every library and four hours, which turned out to be the harder deal: once scipy will solve anything, the question becomes whether you know what you asked it to solve.

---

## 1. Secrecy by Structure (Q2)

This problem came out of 2 semesters spent inside finite fields: Diffie-Hellman, ElGamal, RSA, and eventually elliptic curves. The aim was to hand a room of engineers the central idea of public-key cryptography without ever saying the word.

The story was a light coat of paint over that idea. It is **1984**. Winston and Julia need to agree on where to meet without ever writing a place down, inside a network that logs every byte. Big Brother is not worried. He has already listed every point on the curve they use, and his reasoning fits in one line of the paper:

> *If the system is finite, it can be known. If it can be known, it can be controlled.*

### 1.1 The Setup

Winston and Julia turn their locations into points on

$$E:\; y^2 \equiv x^3 - 19x + 84 \pmod{23},$$

and the meeting place is the sum of the two, \\(P_M = P_W + P_J\\), using the curve's chord-and-tangent rule (elaborately explained in the paper). The curve has 19 points plus a point at infinity, \\(\mathcal{O}\\), which acts as zero. That makes twenty in all, small enough for Big Brother to list.

The question asks you to draw the line on the real curve and then do the arithmetic mod 23. That works because the construction reduces to a few fractions in the coordinates:

$$\lambda = \frac{y_2 - y_1}{x_2 - x_1} \;\;\text{or}\;\; \frac{3x_1^2 + a}{2y_1}, \qquad x_3 = \lambda^2 - x_1 - x_2, \qquad y_3 = \lambda(x_1 - x_3) - y_1,$$

and since 23 is prime, every nonzero number has an inverse, \\(d^{-1} = d^{21}\\). In Round 1 the slash was a loop; here it is an exponent.

```python
p, a = 23, -19

def add(P, Q):
    if P is None: return Q
    if Q is None: return P
    (x1, y1), (x2, y2) = P, Q
    if x1 == x2 and (y1 + y2) % p == 0:
        return None
    if P != Q:
        lam = (y2 - y1)*pow(x2 - x1, p - 2, p) % p
    else:
        lam = (3*x1*x1 + a)*pow(2*y1, p - 2, p) % p
    x3 = (lam*lam - x1 - x2) % p
    return (x3, (lam*(x1 - x3) - y1) % p)
```

### 1.2 The Two Parts

**Part 2** gives both locations, \\((3, 10)\\) and \\((18, 13)\\), and asks for the meeting point. It is \\((14, 20)\\), one call to `add`. Once the locations leak, Big Brother wins.

**Part 1** gives only the meeting point, \\((16, 9)\\), and asks where they were. There is no unique answer. Every point \\(P\\) on the curve pairs with \\((16, 9) - P\\), so twenty different pairs fit. Julia knows her own point, so she gets Winston's with one subtraction (adding the mirror image \\((x, -y)\\)). An outsider sees one point and twenty possible pasts.

That is what the question was built around. Information from two people is merged into one object, in a closed space where every operation stays inside, and it is easy to split only if you already hold one of the pieces.

### 1.3 Making the World Big

At \\(p = 23\\), Big Brother lists everything. Real systems keep the same rule, make the world enormous, and hide a count instead of a point: publish \\(Q = kG\\), keep \\(k\\). Computing \\(kG\\) takes about \\(\log_2 k\\) doublings. Getting \\(k\\) back from \\(Q\\) is the elliptic-curve discrete logarithm problem, and the best general attacks need around \\(2^{128}\\) steps on a 256-bit curve. Diffie–Hellman follows directly: Winston sends \\(aG\\), Julia sends \\(bG\\), and both compute \\(abG\\), which Big Brother cannot.

```python
def mul(k, P):
    R = None
    while k:
        if k & 1: R = add(R, P)
        P = add(P, P)
        k >>= 1
    return R

G = (3, 10)
A, B = mul(7, G), mul(13, G)
assert mul(7, B) == mul(13, A)
```

The world is still closed, the rule still fits in three lines, and the space is simply too large to list. Not secrecy by hiding, but secrecy by structure.

If this stays with you, the next step is Hoffstein, Pipher and Silverman, *An Introduction to Mathematical Cryptography* (2014).

---

## 2. Firm, Fair, Forgiving (Q1)

The Iterated Prisoner's Dilemma is Robert Axelrod's, and the question said so. It sets seven strategies (cooperators, cheats, and reciprocators in between) in a 200-round round-robin, with one change that matters: every move has a 5% chance of being flipped by accident.

**Noise was the question.** Without it, Tit-for-Tat and Grim Trigger behave identically against anyone polite, and the tournament teaches nothing. With noise, a single accident separates the forgiving from the vindictive.

**The payoffs needed one more inequality than the paper stated.** \\(T > R > P > S\\) makes defection tempting, but the iterated game also needs \\(2R > T + S\\). Otherwise two players who take turns exploiting each other beat steady cooperation, and the dilemma dissolves. The standard choice \\((5, 3, 1, 0)\\) satisfies both.

**What noise does to reciprocity** is easiest to see in self-play:

* **Tit-for-Tat vs. itself:** one accidental D starts an echo, \\((C,D) \to (D,C) \to (C,D) \cdots\\), that lasts until another error breaks it. Over a long match the average drifts towards \\((R+T+S+P)/4 = 2.25\\). The simulation gives 2.29.
* **Win–Stay, Lose–Shift vs. itself:** heals in two moves, \\((C,D) \to (D,D) \to (C,C)\\). The player who was exploited switches; the one who got away with it stays; mutual punishment then makes both switch back. Self-play: 2.79.
* **Grim Trigger vs. itself:** the first accident starts a war that never ends. Self-play: 1.25.

**The ranking table lies a little,** which is why the paper also asked for a heat map. In a 5% run with Always Cooperate and the Periodic Defector in the field, Grim and Always Defect top the averaged table (about 2.42), simply by feeding on the naive strategies, while scoring worst of all against their own kind. At 2% noise, Win–Stay, Lose–Shift came out on top. There is no best strategy, only a best strategy for a given population. Axelrod's real result was about whom you keep meeting.

The paper closed on a line meant to outlast the problem: in a noisy world, the strategies that do well are rarely the most aggressive or the most naive, but the ones that are firm, fair and forgiving. That holds wherever people meet again. For a longer look at the same story, Veritasium's [video on the topic](https://www.youtube.com/watch?v=mScpHTIi-kM) is worth the time.

---

## 3. Faith in Constraints (Q3)

An underwater vehicle with no map, intermittent contact and little onboard computation has to cross a cave. The question's real content was a single guarantee, offered almost as an aside: *every internal wall is connected to the boundary.* With no free-standing islands to circle forever, keeping one hand on the wall eventually sweeps every corridor. Breadth-first search solves it as well, but it needs a map and memory. The guarantee is what lets local sensing alone succeed. The hint was the algorithm.

<!-- Q8 (Beautiful particles): to be written with co-designer's input -->

---

Round 1 asked whether a loop settles. Round 2 asked what a closed world allows: you can count it, you can control it, and if it is large enough, you can hide in it.
