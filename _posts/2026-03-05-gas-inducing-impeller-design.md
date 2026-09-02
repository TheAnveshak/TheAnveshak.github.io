---
title: "Building a Gas-Inducing Impeller (and Winning With It)"
date: 2026-03-05
permalink: /posts/2026/03/gas-inducing-impeller-design/
excerpt: A dead-end reactor with internal gas recycle, and then the awkward addition of solid suspension. The design sits at the intersection of two very different hydrodynamic requirements.
tags:
  - Multiphase Transport
  - Fluid Dynamics
  - Reactor Engineering
  - Prototyping
---

Say you want to react a gas into a liquid -- \\(\text{H}_2\\) for a reduction, \\(\text{Cl}_2\\) for a chlorination. Bubble it through a sparger, right?

Except phase equilibrium won't let you finish the job. At the interface, \\(y_i P = H_i x_i\\). Unless \\(H_i = 0\\) (never) or the liquid-phase reaction is infinitely fast (Hatta \\(\to \infty\\)), some gas always survives the bubble's rise and escapes into the headspace unreacted. A single pass can't strip a gas to zero.

If that gas is air, vent it and move on. If it's \\(\text{H}_2\\) or \\(\text{Cl}_2\\), venting is money and a hazard leaving through the roof.

So you recycle the unreacted gas. Simplest fix: catch the vent stream and compress it back to the bottom. Except now you own a compressor -- and if the gas is Chlorine or Phosgene, "compressor room" is not somewhere you want your name on the shift roster.

Fine, skip compressing the gas -- pump the liquid instead, drive a second circulation loop through the headspace. Except now you've got a submerged pump sitting in corrosive slurry, and its seals and bearings are on a maintenance calendar you didn't want.

Both fixes fail the same way: they treat gas recycle as something bolted onto the reactor. What if the reactor did it to itself? Don't add a pump -- let the impeller be the pump, and let the whole vessel become one closed control volume. Gas leaves the liquid at the top, gets pulled back down through the impeller, and re-enters the same volume it left. Nothing crosses the vessel wall. No external loop, no second machine, no seal to fail. 

That's a **Gas-Inducing Contactor (GIC)** -- a dead-end system by design.

Depending on how gas and liquid enter and leave the impeller zone, GICs split into three families: **type 11** (gas only, in and out -- a bare hollow pipe), **type 12** (gas in, gas-liquid dispersion out -- most flotation cells), and **type 22** (two-phase in and out, usually with a stator and standpipe around the impeller). Each trades gas-induction rate against shear, dispersion quality, and mechanical complexity.

Here's the catch: most gas-inducing impellers are terrible solid suspenders. Sit near the surface to keep the induction path short, and the liquid near the tank floor barely moves -- any catalyst or reactant slurry just sits there. The usual fix is a second impeller lower down, purely for suspension, running on the same shaft.

So we built one. Reactor Alchemy, hosted by Tinkerers' Lab and IIC ICT Mumbai -- design and fabricate a gas-inducing impeller under 90 mm, in a 150 mm tank, that suspends 2–3 mm solids and induces gas, tested against \\(\text{CO}_2\\)–\\(\text{NaOH}\\) absorption.

![All six functioning impellers from Reactor Alchemy](/images/reactor-alchemy-teams.png)
*Figure 4.1: From left to right -- Team 3, Team 4, Team 5, Team 2, Team 6, Team 1.*

Ours held. A 92 mm hollow-shaft impeller with pitched blades, single geometry doing both jobs as above. Gas induction switched on at \\(N_{CG} = 368\\) RPM; solid suspension followed at \\(N_{CS} = 500\\) RPM -- consistent with the standing assumption in this class of contactor that \\(N_{CS} > N_{CG}\\), since a settled bed needs more liquid momentum near the tank floor than a headspace vortex needs to reach the impeller tip. At that suspension speed, absorption ran at \\(1.26\times10^{-3}\ \text{mol L}^{-1}\text{s}^{-1}\\) drawing 123 W, giving a mass-transfer-to-power ratio of \\(1.02\times10^{-5}\ \text{mol L}^{-1}\text{s}^{-1}\text{W}^{-1}\\) -- 1.8\\(\times\\) the next-best team.

That margin is the honest story, not the absolute rate. Literature is unambiguous that a dedicated second impeller beats a single hollow-shaft design on raw gas-induction rate and dispersion quality -- Saravanan and Joshi's multiple-impeller data shows exactly that gap. We weren't chasing peak \\(Q_G\\). By collapsing induction and suspension onto one blade, we cut shaft power and mechanical complexity, and the competition scored efficiency, not throughput. First place came from picking the metric that rewarded doing less, not doing more -- a fair result, but one that would likely lose on a rig scored purely on absorption rate.

---

### References

* **Saravanan, K., Patwardhan, A. W., & Joshi, J. B. (1997).** Critical Impeller Speed for Solid Suspension in Gas Inducing Type Mechanically Agitated Contactors. *The Canadian Journal of Chemical Engineering*, 75(4), 664–676.
* **Patwardhan, A. W., & Joshi, J. B. (1999).** Design of Gas-Inducing Reactors. *Industrial & Engineering Chemistry Research*, 38(1), 49–80.
