---
title: "Building a Gas-Inducing Impeller (and Winning With It)"
date: 2026-03-05
permalink: /posts/2026/03/gas-inducing-impeller-design/
excerpt: A dead-end reactor with internal gas recycle, and then the addition of solids. The design sits at the intersection of two very different hydrodynamic requirements.
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

So we built one for Reactor Alchemy, hosted by Tinkerers' Lab and IIC ICT Mumbai in partnership with Amar Equipment. Amar is fundamentally a process equipment design and fabrication outfit, nobody cared if we could cite drag correlations or calculate a Sauter mean diameter on paper. They handed us a 150 mm acrylic tank and wanted a sub-90 mm rotor that simply worked: induce gas from the headspace, suspend 2–3 mm solids, and spin at 500 RPM without shaking the entire rig to pieces, tested against \\(\text{CO}_2\\)-\\(\text{NaOH}\\) absorption. The work came down to basic mechanical constraints: holding \\(C_4\\)-axis symmetry, pulling leak-tight hollow passages off an FDM printer, and balancing blade mass so the shaft wouldn't whip.

![All six functioning impellers from Reactor Alchemy](/images/reactor-alchemy-teams.png)\\
*Figure 4.1: From left to right -- Team 3, Team 4, Team 5, Team 2, Team 6, Team 1.*

### The Theory & Fabrication Disconnect
In a transport phenomena exam, multiphase contactor design is trivial: assume a constant Sauter mean diameter \\(d_{32}\\), pull a single-bubble drag coefficient from Schiller-Naumann or Kumar-Hartland, ignore population balance breakup/coalescence kernels, and plug into an empirical $k_L a$ correlation.  

![All six functioning impellers from Reactor Alchemy](/images/GIC_git/just_GIC_3baldes.jpeg)\\
*Figure 4.2: The naive additive approach.*
The vertical tubes on the Fracktal bed Figure 4.2 printed by my teammate Waqqas for his own project—served as a reference, though pure hollow pipes have essentially zero solid-suspension capability. Vertical prints snapped along layer lines, supports clogged the hollow bore, and polypropylene warped off the bed until we switched to ABS+.

![All six functioning impellers from Reactor Alchemy](/images/GIC_git/iteration2_blades.jpeg)
![All six functioning impellers from Reactor Alchemy](/images/GIC_git/iteration2_design_side_45.jpeg)\\
*Figure 4.2: The Machined Acrylic Cavity & the Assembled Prototype.*

To bypass the internal support problem, our first post-initial build was a laminated three-layer acrylic sandwich: front face, channeled manifold core, and back plate. We built the entire unit, which served as the testbed for our first four-face interlocking hub-a collar that clamped directly onto the hollow Astral CPVC shaft while mechanically socketing four blades at $90^\circ$ to enforce \\(C_4\\) axis symmetry.The assembly held together, but the design carried fatal mechanical penalties. The solid-to-void ratio was terrible; laminating solid acrylic sheets packed excessive dead weight at the outer radius, unnecessarily driving up rotational inertia (\\(I = \sum m_i r_i^2\\) ) and mechanical power draw while forcing the hub to cantilever severe cyclic bending moments. On top of that, hand-machined channels between bonded sheets gave poor dimensional consistency. It proved the interlocking hub kinematics could constrain the hollow axis and blades, but the peripheral mass and fabrication tolerances forced us to ditch the three-layer acrylic stack entirely and move to the lightweight open-trough hybrid shell

Fabrication turned out to be the actual competition. Design a blade profile in CAD and it's clean vector math. Print it, and reality intervenes.

We wanted the impeller light, so ABS on FDM. First problem: the printer available couldn't do the hollow internal bore the shaft needed -- support material inside a thin tube doesn't clear, and the holes on the rear face of the blade (where induced gas exits into the bulk) kept printing warped or half-closed. The shaft-to-impeller joint, which has to stay hollow all the way through, came out either blocked or structurally weak.

Fix: flip the print orientation. Lay the blade flat with the leading face down, so the "roof" over the shaft joint -- the one hollow passage that actually mattered, printed clean and unsupported. Trade-off: the rear face of the blade, with its exit holes, couldn't print this way at all. So we didn't print it. We CNC-cut a flat acrylic sheet with the holes machined in, and glued it onto the back of the printed part as a cap. Worked better than either method alone would have.

That solved the blade. It didn't solve the impeller staying on-axis. A hollow shaft carrying an off-axis pitched blade wants to wobble and drift under its own weight distribution. We built a hub, a self-locking collar fixed to the shaft, with sockets the blades slotted and locked into, so every blade sat pinned at a fixed radius and angle instead of relying on a single glued joint to hold alignment. 
Once the geometry was fixed and load-bearing, the last problem was leaks -- every printed seam and glued joint is a path for induced gas to escape before it reaches the liquid. Hot glue, run along every seal line and balanced the last bit of asymmetric weight.

Ours held. A 92 mm hollow-shaft impeller with pitched blades, single geometry doing both jobs as above. Gas induction switched on at \\(N_{CG} = 368\\) RPM; solid suspension followed at \\(N_{CS} = 500\\) RPM -- consistent with the standing assumption in this class of contactor that \\(N_{CS} > N_{CG}\\), since a settled bed needs more liquid momentum near the tank floor than a headspace vortex needs to reach the impeller tip. At that suspension speed, absorption ran at \\(1.26\times10^{-3}\ \text{mol L}^{-1}\text{s}^{-1}\\) drawing 123 W, giving a mass-transfer-to-power ratio of \\(1.02\times10^{-5}\ \text{mol L}^{-1}\text{s}^{-1}\text{W}^{-1}\\) -- 1.8\\(\times\\) the next-best team.

That margin is the honest story, not the absolute rate. Literature is unambiguous that a dedicated second impeller beats a single hollow-shaft design on raw gas-induction rate and dispersion quality -- Saravanan and Joshi's multiple-impeller data shows exactly that gap. We weren't chasing peak \\(Q_G\\). By collapsing induction and suspension onto one blade, we cut shaft power and mechanical complexity, and the competition scored efficiency, not throughput. First place came from picking the metric that rewarded doing less, not doing more -- a fair result, but one that would likely lose on a rig scored purely on absorption rate.

---

### References

* **Saravanan, K., Patwardhan, A. W., & Joshi, J. B. (1997).** Critical Impeller Speed for Solid Suspension in Gas Inducing Type Mechanically Agitated Contactors. *The Canadian Journal of Chemical Engineering*, 75(4), 664–676.
* **Patwardhan, A. W., & Joshi, J. B. (1999).** Design of Gas-Inducing Reactors. *Industrial & Engineering Chemistry Research*, 38(1), 49–80.
