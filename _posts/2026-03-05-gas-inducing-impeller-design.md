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
In a transport phenomena exam, multiphase contactor design is trivial: assume a constant Sauter mean diameter \\(d_{32}\\), pull a single-bubble drag coefficient from Schiller-Naumann or Kumar-Hartland, ignore population balance breakup/coalescence kernels, and plug into an empirical \\(k_L a\\) correlation.  

![All six functioning impellers from Reactor Alchemy](/images/GIC_git/just_GIC_3baldes.jpeg)\\
*Figure 4.2: The naive additive approach.*
The vertical tubes on the Fracktal bed Figure 4.2 printed by my teammate Waqqas for his own project—served as a reference, though pure hollow pipes have essentially zero solid-suspension capability. Vertical prints snapped along layer lines, supports clogged the hollow bore, and polypropylene warped off the bed until we switched to ABS+.

<div style="display: flex; gap: 10px; justify-content: center; align-items: center;">
  <img src="/images/GIC_git/iteration2_blades.jpeg" alt="Machined Acrylic Cavity" style="width: 49%; max-height: 280px; object-fit: cover;">
  <img src="/images/GIC_git/iteration2_design_side_45.jpeg" alt="Assembled Prototype" style="width: 49%; max-height: 280px; object-fit: cover;">
</div>

*Figure 4.3: The machined acrylic manifold cavity (left) and the assembled four-face collar prototype (right).*

To bypass the internal support problem, our first post-initial build was a laminated three-layer acrylic sandwich: front face, channeled manifold core, and back plate. We built the entire unit, which served as the testbed for our first four-face interlocking hub-a collar that clamped directly onto the hollow Astral CPVC shaft while mechanically socketing four blades at \\(90^\circ\\) to enforce \\(C_4\\) axis symmetry.The assembly held together, but the design carried fatal mechanical penalties. The solid-to-void ratio was terrible; laminating solid acrylic sheets packed excessive dead weight at the outer radius, unnecessarily driving up rotational inertia (\\(I = \sum m_i r_i^2\\) ) and mechanical power draw while forcing the hub to cantilever severe cyclic bending moments. On top of that, hand-machined channels between bonded sheets gave poor dimensional consistency. It proved the interlocking hub kinematics could constrain the hollow axis and blades, but the peripheral mass and fabrication tolerances forced us to ditch the three-layer acrylic stack entirely and move to the lightweight open-trough hybrid shell

<div style="display: flex; gap: 10px; justify-content: center; align-items: center;">
  <img src="/images/GIC_git/main_iteration_view_2.jpeg" alt="Open printed trough" style="width: 49%;">
  <img src="/images/GIC_git/main_iteration_few_blades.jpeg" alt="Blade capping evolution" style="width: 49%;">
</div>

*Figure 4.4: The open-trough structural shell (left) and the progression of capping the trailing edge with drilled acrylic plates (right).*

We wanted low rotational inertia, which pointed straight back to printing in ABS+. But a closed hollow blade presented the exact same barrier we hit with the vertical tubes: you cannot clear support material out of a 2 mm internal gas bore, and printing horizontal hollow overhangs without support leaves sagged, bridging filament that chokes the suction path. If you try to print exit orifices directly onto the blade face, extrusion pressure distorts them into half-closed slits.

The fix was a fundamental change in manufacturing logic: do not print a closed volume.

We flipped the print orientation entirely. We laid the blade flat and printed it as an open structural trough with the leading face down against the bed. This solved two failure modes simultaneously:The high-pressure leading face printed glassy-smooth with continuous $x$-$y$ filament rastering, giving it high flexural rigidity against hydrodynamic drag without layer-boundary weakness.The hollow internal manifold printed clean and completely unsupported as an open cavity.The trade-off was that the rear face couldn't be closed on the printer. So we didn't try. We CNC-cut flat acrylic sheets to match the trough profile, drilled four discrete exit holes along the span, and solvent-welded them onto the back of the printed shell as a cap.

The flat acrylic cap sat directly in the low-pressure wake behind the pitched blade—exploiting the localized pressure drop documented by Martin (1972) and Evans et al. (1990) to maximize induction driving force—while the internal volume stayed completely unobstructed

We replaced butt-glued joints with an indexed central hub collar. The hub was fabricated with four mortise sockets arrayed at precise \\(90^\circ\\) radial offsets, each broached at a \\(45^\circ\\) downflow pitch. The printed blades featured matching tenons that slotted into these sockets and pinned mechanically. We pressed this hub onto a section of rigid, off-the-shelf Astral Class 1 SDR-11 CPVC pipe.

![All six functioning impellers from Reactor Alchemy](/images/GIC_git/main_impeller_Side.jpeg)
*Figure 4.5: Team 3 **WInning** Prototype.*

Ours held. A 92 mm hollow-shaft impeller with pitched blades, single geometry doing both jobs as above. Gas induction switched on at \\(N_{CG} = 368\\) RPM; solid suspension followed at \\(N_{CS} = 500\\) RPM -- consistent with the standing assumption in this class of contactor that \\(N_{CS} > N_{CG}\\), since a settled bed needs more liquid momentum near the tank floor than a headspace vortex needs to reach the impeller tip. At that suspension speed, absorption ran at \\(1.26\times10^{-3}\ \text{mol L}^{-1}\text{s}^{-1}\\) drawing 123 W, giving a mass-transfer-to-power ratio of \\(1.02\times10^{-5}\ \text{mol L}^{-1}\text{s}^{-1}\text{W}^{-1}\\) -- 1.8\\(\times\\) the next-best team.

That margin is the honest story, not the absolute rate. Literature is unambiguous that a dedicated second impeller beats a single hollow-shaft design on raw gas-induction rate and dispersion quality -- Saravanan and Joshi's multiple-impeller data shows exactly that gap. We weren't chasing peak \\(Q_G\\). By collapsing induction and suspension onto one blade, we cut shaft power and mechanical complexity, and the competition scored efficiency, not throughput. First place came from picking the metric that rewarded doing less, not doing more -- a fair result, but one that would likely lose on a rig scored purely on absorption rate.

---

### References

* **Saravanan, K., Patwardhan, A. W., & Joshi, J. B. (1997).** Critical Impeller Speed for Solid Suspension in Gas Inducing Type Mechanically Agitated Contactors. *The Canadian Journal of Chemical Engineering*, 75(4), 664–676.
* **Patwardhan, A. W., & Joshi, J. B. (1999).** Design of Gas-Inducing Reactors. *Industrial & Engineering Chemistry Research*, 38(1), 49–80.
  * **Martin, G. Q. (1972).** Gas-Inducing Agitator. *Industrial & Engineering Chemistry Process Design and Development*, 11(3), 397–404.
* **Evans, G. M., Rielly, C. D., Davidson, J. F., & Carpenter, K. J. (1990).** A Fundamental Study of Gas Inducing Impeller Design. *Institution of Chemical Engineers Symposium Series*, 121, 137–152.
