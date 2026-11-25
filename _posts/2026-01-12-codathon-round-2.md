---
title: "Finite Is Not Small"
date: 2026-11-25
permalink: /posts/2026/11/codathon-round-2/
excerpt: Notes on Round 2 of Vortex Codathon 13.0 -- an elliptic curve over a field of twenty-three numbers, a noisy tournament of cooperators and cheats, and a maze that only needed faith in its own guarantees.
tags:
  - cryptography
  - game-theory
  - scientific-computing
---

> **Round 2 Artifacts:**  
> * **Problem Paper:** [Round 2 PDF]({{ base_path }}/files/Codathon_R2.pdf)
> * **Source Repository:** [`Vortex-ICT-Mumbai/Vortex_13.0/events/prodigy/codathon`](https://github.com/Vortex-ICT-Mumbai/Vortex_13.0/tree/main/events/prodigy/codathon)
> * **Previously:** [The Slash Was Always a Loop](/posts/2026/11/codathon-round-1/) (Round 1) · [Designing Vortex Codathon](/posts/2026/01/vortex-codathon/)

<!-- MORE BUTTON: Anything below this line only shows when the user clicks the post -->
<!--more-->

---

Round 1 took the tools away. Round 2 handed them back, allowed four hours, and asked what a solver would build with them. The answers covered curves, games, mazes and reactors. Two of the eight problems carried most of the weight.

---

## 1. Secrecy by Structure (Q2)

This problem came out of a summer spent inside finite fields: Diffie–Hellman, ElGamal, RSA, and eventually elliptic curves. The aim was to hand a room of engineers the central idea of public-key cryptography without ever saying the word.

The story was a light coat of paint over that idea. It is 1984. Winston and Julia hide messages from Big Brother by stamping "69" into the logs, until the suits notice that \\(69 = 3 \times 23\\). The joke already contains the prime the whole problem lives on. Big Brother's philosophy fits in one line of the paper:

> *If the system is finite, it can be known. If it can be known, it can be controlled.*

The rest of the question tests that sentence and then breaks it.

### 1.1 A World of Twenty Points

Everything happens on

$$E:\; y^2 \equiv x^3 - 19x + 84 \pmod{23},$$

which over \\(\mathbb{F}_{23}\\) is \\(y^2 = x^3 + 4x + 15\\). It has 19 points with coordinates plus one point at infinity, \\(\mathcal{O}\\), which acts as zero: 20 points in all. That falls inside Hasse's bound, \\(\lvert \#E - (p+1) \rvert \leq 2\sqrt{p}\\), which allows anything from 15 to 33 for \\(p = 23\\). The group happens to be cyclic: \\((3, 10)\\) on its own generates all twenty.

### 1.2 Draw in \\(\mathbb{R}^2\\), Compute in \\(\mathbb{F}_{23}\\)

The paper insisted on one apparent inconsistency: picture the chord-and-tangent rule on the real curve, but evaluate it on the grid. Nothing breaks, because the geometric rule boils down to rational functions of the coordinates:

$$\lambda = \frac{y_2 - y_1}{x_2 - x_1} \;\;\text{or}\;\; \frac{3x_1^2 + a}{2y_1}, \qquad x_3 = \lambda^2 - x_1 - x_2, \qquad y_3 = \lambda(x_1 - x_3) - y_1.$$

Only \\(+, -, \times, \div\\) appear, so the formulas hold in any field. The one place a field earns its name is the division. Because 23 is prime, every nonzero \\(d\\) has an inverse, \\(d^{-1} = d^{p-2}\\) (Fermat). In Round 1 the slash was a loop; here it is an exponent.

Primality is not decoration. Modulo a composite \\(n\\), some denominators have no inverse, and the rule fails. Lenstra turned that failure into a factoring algorithm: a slope that refuses to invert hands you a factor of \\(n\\).

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

### 1.3 The Two Parts, and What They Were For

**Part 2** is mechanical: \\((3, 10) + (18, 13) = (14, 20)\\). If Winston and Julia's coordinates leak, Big Brother finds the meeting point in one line.

**Part 1** gives the oracle's output, \\((16, 9)\\), and asks where the two of them were. It has no single answer, and that is the answer. For *every* point \\(P\\) on the curve, \\(P + \big((16,9) - P\big) = (16, 9)\\), so there are exactly twenty equally valid stories. Julia, knowing her own point, recovers Winston's in one subtraction: \\(P_W = P_M - P_J\\), where subtracting means adding the mirror image \\((x, -y)\\). Anyone outside sees one point and twenty possible pasts.

That was the closing thought the paper left on the page: information from two people merged into one object, living in a closed space, where every operation stays inside, and which is easy to separate only if you already hold one of the parts.

### 1.4 From Toy to Shield

The toy is honest about one thing and quiet about another. Adding a secret point is a one-time pad on a group. It hides perfectly because of *ambiguity*, not difficulty, and collapses the moment one part leaks. Real systems hide a *count* instead of a point:

$$Q = kG = \underbrace{G + G + \cdots + G}_{k \text{ times}}.$$

Going forward is cheap: double-and-add reaches \\(kG\\) in about \\(\log_2 k\\) steps. Going backward, recovering \\(k\\) from \\(Q\\), is the elliptic-curve discrete logarithm problem. The best generic attacks take about \\(\sqrt{n}\\) group operations, so a 256-bit curve resists at roughly \\(2^{128}\\). Diffie–Hellman is then three lines: Winston publishes \\(aG\\), Julia publishes \\(bG\\), and both compute \\(abG\\), which Big Brother cannot form from what he sees.

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

At \\(p = 23\\), Big Brother lists all twenty points and always wins. At 256 bits, the world is still finite, still closed, and still obeys the same three-line rule. It is simply too large to list. Not secrecy by hiding, but secrecy by structure. That is also why a 256-bit curve does the work of a 3072-bit RSA modulus.

For anyone the feeling stays with, the paper pointed to Hoffstein, Pipher and Silverman, *An Introduction to Mathematical Cryptography* (2014).

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
