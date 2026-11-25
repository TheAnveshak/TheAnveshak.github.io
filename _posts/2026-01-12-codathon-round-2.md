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

Round 1 banned the libraries. Round 2 allowed every library and four hours, which turned out to be the harder deal: once `scipy` will solve anything, the question becomes whether you know what you asked it to solve.

---

## 1. Secrecy by Structure (Q2)

I spent two semesters inside finite fields: Diffie–Hellman, ElGamal, RSA, and eventually elliptic curves. Somewhere along the way I wanted to see if the core idea of public-key cryptography could be handed to a room of chemical engineers without ever saying the word.

It's 1984. Winston and Julia need to agree on a place to meet, inside a network that logs every byte, without ever writing a place down. Big Brother isn't worried. His whole philosophy is one line:

> *If the system is finite, it can be known. If it can be known, it can be controlled.*

So each of them turns their location into a point on

$$E:\; y^2 \equiv x^3 - 19x + 84 \pmod{23},$$

and the meeting place is the sum of the two points, \\(P_M = P_W + P_J\\). "Sum" means the chord-and-tangent rule: draw the line through both points, find where it hits the curve again, flip it. You picture it on the real curve and compute it mod 23. The only step that needs a second thought is the division in the slope, and since 23 is prime, \\(d^{-1} = d^{21}\\). In Round 1 the slash was a loop; here it's an exponent.

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

The whole curve is 19 points plus a point at infinity that behaves like zero. Twenty points. Big Brother can write them all on one page.

### 1.1 Two Directions

Forward is trivial. If you know both locations, say \\((3, 10)\\) and \\((18, 13)\\), the meeting point is \\((14, 20)\\), one call to `add`.

Backward is where it gets interesting. Give someone only the meeting point, \\((16, 9)\\), and ask where the two of them were. They can't say. Every point \\(P\\) on the curve pairs with \\((16, 9) - P\\), so all twenty pairs fit equally well. Julia, though, knows her own point, and for her it's one subtraction: \\(P_W = P_M - P_J\\), where minus just means adding the mirror image \\((x, -y)\\).

Sit with that for a second. Two people's information is merged into one object. The object lives in a closed world where every operation lands back inside. And pulling it apart is easy only if you already hold one of the pieces. You've seen this before, just not on graph paper: in locks, handshakes, and agreements made in public that only two parties understand.

### 1.2 Bigggg but Finite

At \\(p = 23\\), Big Brother lists everything and always wins. So make the world huge. Real systems keep exactly the same rule and hide a count instead of a point: publish \\(Q = kG\\), keep \\(k\\). Getting from \\(k\\) to \\(Q\\) takes about \\(\log_2 k\\) doublings. Getting from \\(Q\\) back to \\(k\\) on a 256-bit curve takes something like \\(2^{128}\\) steps with the best general attacks anyone knows. After that, Diffie–Hellman is almost a footnote: Winston sends \\(aG\\), Julia sends \\(bG\\), both compute \\(abG\\), and Big Brother, holding only \\(aG\\) and \\(bG\\), is stuck.

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

Same closed world, same few lines of rule. Not magic, not secrecy by hiding. Secrecy by structure. The world is still finite; it's just too big to list.

If this one stays with you, Hoffstein, Pipher and Silverman's *An Introduction to Mathematical Cryptography* (2014) is a gentle but serious next step.

---

## 2. Firm, Fair, Forgiving (Q1)

This one is Robert Axelrod's; I'm only borrowing it. Should I cooperate, or look after myself? Vampire bats share blood with whoever failed to feed that night, but only with bats that return the favour. Arms treaties hold until one side cheats. Group projects, well, you know. The Prisoner's Dilemma squeezes that tension into four numbers, and the iterated version asks what happens when you keep meeting the same people.

Seven strategies, everyone against everyone (including themselves), 200 rounds per match. The one twist I cared about: every move has a 5% chance of being flipped by accident. A message misread, a button pressed wrong.

**Noise is the whole thing.** Without it, Tit-for-Tat and Grim Trigger look identical against anyone polite. With it, a single accident shows you who forgives and who doesn't.

**A small trap in the payoffs.** \\(T > R > P > S\\) isn't quite enough. You also want \\(2R > T + S\\); otherwise two players taking turns exploiting each other outscore honest cooperation, and the dilemma quietly disappears. The usual \\((5, 3, 1, 0)\\) passes both.

It's easiest to see what noise does when a strategy plays a copy of itself:

* **Tit-for-Tat:** one accidental D starts an echo, \\((C,D) \to (D,C) \to (C,D) \cdots\\), that runs until another error breaks it. Over a long match it drifts towards \\((R+T+S+P)/4 = 2.25\\); my run gave 2.29.
* **Win–Stay, Lose–Shift:** heals in two moves, \\((C,D) \to (D,D) \to (C,C)\\). The one who got exploited switches, the one who got away with it stays, and then mutual punishment flips both back. 2.79.
* **Grim Trigger:** the first accident starts a war that never ends. 1.25.

**The ranking table lies a little,** which is why a heat map matters. At 5% noise, with Always Cooperate and a Periodic Defector in the field, Grim and Always Defect top the averaged table (about 2.42) simply by feeding on the naive ones, while scoring worst against their own kind. At 2% noise, Win–Stay, Lose–Shift came out on top. There's no best strategy, only a best strategy for the people around you.

In a noisy world, the ones that do well are rarely the most aggressive or the most naive, but the ones that are firm, fair and forgiving. It was never really about this problem; it's about the social decisions you make when you keep meeting the same people. Veritasium has a [great video](https://www.youtube.com/watch?v=mScpHTIi-kM) on the same story.

---

## 3. If It Were Obvious, It Wouldn't Be Engineering (Q5)

Ammonia oxidation sits at the heart of the Ostwald process. It moves millions of tonnes of material a year, releases enormous heat, destroys expensive catalysts, and quietly prints money. Your commerce friend looks at the overall reaction, sees ammonia go in and nitric acid come out, and smiles. *"This is trivial. Raw material in, product out, margins look great. Scale it. IPO it."*

You, unfortunately, know better. Before a single reactor is sized, before heat removal is designed, before safety margins are set, you don't even know what's happening inside the box. The gas phase is violent, residence times are short, the catalyst degrades, and most intermediates are never observed. And yet the plant must be built.

So this one runs backwards. You get a table of concentrations for a handful of nitrogen species and nothing else. Propose series and parallel rate laws, fit the constants, and show the model reproduces the data. There's no unique answer, only fit quality and whether the logic holds.

The table did come from a mechanism. I wrote one, integrated it, kept every eleventh point, rounded to four decimals, and threw the mechanism away. Ten rows are all that's left of it.

What I hoped people would notice is how much you can read before fitting anything. B forms only in the first step and is never consumed, so its plateau, 3.074, counts exactly how many times that step ran. Each run eats three A, so about 9.22 of the starting 10 went that way, and the remaining 0.78 went down the competing branch. That's the branching ratio straight off the table, with no optimizer involved. A model that misses that number is wrong however small its MSE.

This is the mechanism the ten rows were cut from:

![Concentration profiles of the hidden mechanism]({{ base_path }}/images/codathon_r2_q5.png)

Get the structure right and the fit is almost an afterthought. Starting every rate constant at 1, least squares walks straight back to the ones I hid, \\(k = (7.00, 3.00, 2.00)\\), with \\(R^2\\) indistinguishable from 1.

Get the structure wrong and the optimizer will still hand you numbers, just numbers for a reactor that doesn't exist. With only ten rows, a few different mechanisms can fit almost equally well. That isn't a flaw in the problem; that's what kinetics actually feels like.

---

## 4. Three Answers, One Reactor (Q6)

Three reversible gas-phase reactions, \\(A + B \rightleftharpoons C + D\\), \\(B + C \rightleftharpoons X + Y\\) and \\(A + X \rightleftharpoons Z\\), collapse into three nonlinear equations in \\(C_D, C_X, C_Z\\). Solving them is routine. The catch was solving them three times, from initial guesses of 0, 1 and 10:

| Initial guess | \\(C_D\\) | \\(C_X\\) | \\(C_Z\\) | Physical? |
|---|---|---|---|---|
| 0 | 0.705 | 0.178 | 0.374 | yes |
| 1 | 0.056 | 0.597 | 1.082 | no, \\(C_B, C_C < 0\\) |
| 10 | 1.070 | \\(-0.323\\) | 1.131 | no, \\(C_A, C_X < 0\\) |

All three satisfy the equations to \\(10^{-10}\\). The algebra can't tell them apart; only the fact that concentrations can't be negative can. A residual near zero proves you found *a* root, not *the* answer.

<!-- Q8 (Beautiful particles): to be written with co-designer's input -->

---

Round 1 asked whether a loop settles. Round 2 asked what a closed world allows: you can count it, you can control it, and if it's large enough, you can hide in it.
