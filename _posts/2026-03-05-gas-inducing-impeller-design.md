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

Say you want to capture or react a gas into a liquid. Bubble it through a sparger, right?

Except phase equilibrium won't let you, at the interface, \\(y_i P = H_i x_i\\). Unless \\(H_i = 0\\) (never) or the liquid-phase reaction is infinitely fast (Hatta \\(\to \infty\\)), some gas always survives the bubble's rise and escapes into the headspace unreacted. A single pass can't strip a gas to zero.
If that gas is air, vent it and move on. If it's \\(\text{H}_2\\) or \\(\text{Cl}_2\\), venting is money and a hazard leaving through the roof.

So you recycle the unreacted gas. Simplest fix: catch the vent stream and compress it back to the bottom. Except now you own a compressor -- and if the gas is Chlorine or Phosgene, "compressor room" is not somewhere you want to be near.

Fine, skip compressing the gas -- pump the liquid instead, drive a second circulation loop through the headspace. Except now you've got a submerged pump sitting in corrosive slurry, and its on a maintenance calendar.

Both fixes fail the same way: they treat gas recycle as something bolted onto the reactor. 
What if the reactor did it to itself? Enter the dead-end reactor: no exit stream, no external machinery. 
The impeller acts as its own pump, sucking headspace gas back down through hollow blades until every mole reacts. 
It is called dead-end by design, nothing crosses the vessel boundary, and without internal recycle, your flowsheet hits one anyway.
Here's the catch: most gas-inducing impellers are terrible solid suspenders. 

So we built one for Reactor Alchemy Competition, hosted by Tinkerers' Lab and IIC ICT Mumbai in partnership with Amar Equipment. AEPL is fundamentally a process equipment design and fabrication company, nobody cared if we could cite drag correlations or calculate a Sauter mean diameter on paper. They handed us a 150 mm acrylic tank and wanted a sub-90 mm rotor that simply worked: induce gas from the headspace, suspend 2–3 mm solids, tested against \\(\text{CO}_2\\)-\\(\text{NaOH}\\) absorption. The work came down to basic mechanical constraints: holding \\(C_4\\)-axis symmetry, pulling leak-tight hollow passages off an FDM printer, and balancing blade mass so the shaft wouldn't whip.

![All six functioning impellers from Reactor Alchemy](/images/reactor-alchemy-teams.png)\\
*Figure 4.1: From left to right -- Team 3, Team 4, Team 5, Team 2, Team 6, Team 1.*

### The Theory & Fabrication Disconnect

![All six functioning impellers from Reactor Alchemy](/images/GIC_git/just_GIC_3baldes.jpeg)\\
*Figure 4.2: The naive additive approach.*

The vertical tubes on the Fracktal bed Figure 4.2 printed by my teammate Waqqas for his own project—served as a reference, though pure hollow pipes have essentially zero solid-suspension capability. Vertical prints snapped along layer lines, supports clogged the hollow bore, and polypropylene warped off the bed until we switched to ABS+.

<div style="display: flex; gap: 10px; justify-content: center; align-items: center;">
  <img src="/images/GIC_git/iteration2_blades.jpeg" alt="Machined Acrylic Cavity" style="width: 49%; max-height: 280px; object-fit: cover;">
  <img src="/images/GIC_git/iteration2_design_side_45.jpeg" alt="Assembled Prototype" style="width: 49%; max-height: 280px; object-fit: cover;">
</div>

*Figure 4.3: The machined acrylic manifold cavity (left) and the assembled four-face collar prototype (right).*

To bypass the internal support problem, our first post-initial build was a laminated three-layer acrylic sandwich: front face, channeled manifold core, and back plate. 
We built the entire unit, which served as the testbed for our first four-face interlocking hub (Arjun's idea) that clamped directly onto the hollow Astral CPVC shaft.
The assembly held together, but the design carried fatal mechanical penalties. 
The solid-to-void ratio was terrible; laminating solid acrylic sheets packed excessive dead weight at the outer radius, unnecessarily driving up rotational inertia (\\(I = \sum m_i r_i^2\\) ) and mechanical power draw.
On top of that, hand-machined channels between bonded sheets gave poor dimensional consistency, this forced us to pivot.

<div style="display: flex; gap: 10px; justify-content: center; align-items: center;">
  <img src="/images/GIC_git/main_iteration_view_2.jpeg" alt="Open printed trough" style="width: 49%;">
  <img src="/images/GIC_git/main_iteration_few_blades.jpeg" alt="Blade capping evolution" style="width: 49%;">
</div>

*Figure 4.4: The open-trough structural shell (left) and the progression of capping the trailing edge with drilled acrylic plates (right).*

We wanted low rotational inertia, which pointed straight back to printing in ABS+. 
But a closed hollow blade presented the exact same barrier we hit with the vertical tubes: you cannot clear support material out of a 2 mm internal gas bore, and printing horizontal hollow overhangs without support leaves sagged, bridging filament that chokes the suction path. 
If you try to print exit orifices directly onto the blade face, extrusion pressure distorts them into half-closed slits.

The fix was a fundamental change in manufacturing logic: **do not print a closed volume**.

We flipped the print orientation entirely. 
We laid the blade flat and printed it as an open structural trough with the leading face down against the bed. This solved two failure modes simultaneously:The high-pressure leading face printed glassy-smooth and the hollow internal manifold printed clean and completely unsupported as an open cavity.
The trade-off was that the rear face couldn't be closed on the printer. 
So we CNC-cut flat acrylic sheets to match the trough profile, along with four discrete exit holes along the span, and glued them onto the back of the printed shell as a cap.

We replaced butt-glued joints with an indexed central hub collar. 
The hub was fabricated with four dovetail sockets, each broached at a \\(45^\circ\\) downflow pitch. 
The printed blades slotted into these sockets. We pressed this hub onto a section of rigid, off-the-shelf Astral Class 1 SDR-11 CPVC pipe.

![All six functioning impellers from Reactor Alchemy](/images/GIC_git/main_impeller_Side.jpeg)
*Figure 4.5: Team 3 **Winning** Prototype.*

Ours held. A 92 mm hollow-shaft impeller with pitched blades. Gas induction switched on at \\(N_{CG} = 368\\) RPM; solid suspension followed at \\(N_{CS} = 500\\) RPM. At that suspension speed, absorption ran at \\(1.26\times10^{-3}\ \text{mol L}^{-1}\text{s}^{-1}\\) drawing \\(123 W\\), giving a mass-transfer-to-power ratio of \\(1.02\times10^{-5}\ \text{mol L}^{-1}\text{s}^{-1}\text{W}^{-1}\\) -- 1.8\\(\times\\) the next-best team.

That margin is the honest story, not the absolute rate, dual impellers objectively crush single hollow shafts on raw $Q_G$, but collapsing both duties onto one blade cut shaft power and mechanical failure points. First place came from picking the metric that rewarded doing less, not doing more -- a fair result.

---

### References

* **Saravanan, K., Patwardhan, A. W., & Joshi, J. B. (1997).** Critical Impeller Speed for Solid Suspension in Gas Inducing Type Mechanically Agitated Contactors. *The Canadian Journal of Chemical Engineering*, 75(4), 664–676.
* **Patwardhan, A. W., & Joshi, J. B. (1999).** Design of Gas-Inducing Reactors. *Industrial & Engineering Chemistry Research*, 38(1), 49–80.
* **Martin, G. Q. (1972).** Gas-Inducing Agitator. *Industrial & Engineering Chemistry Process Design and Development*, 11(3), 397–404.
* **Evans, G. M., Rielly, C. D., Davidson, J. F., & Carpenter, K. J. (1990).** A Fundamental Study of Gas Inducing Impeller Design. *Institution of Chemical Engineers Symposium Series*, 121, 137–152.
