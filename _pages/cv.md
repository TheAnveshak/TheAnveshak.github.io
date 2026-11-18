---
layout: archive
title: "Curriculum Vitae"
permalink: /cv/
author_profile: true
usemathjax: true
redirect_from:
  - /resume
---

{% include base_path %}

<div style="margin-bottom: 2em; display: flex; align-items: center; justify-content: space-between;">
  <span><strong>Arya Rane</strong> --- Academic & Research Record</span>
  <a href="{{ base_path }}/files/AryaRane_3_6.pdf" class="btn btn--primary" target="_blank" style="margin: 0; padding: 0.4em 1em; font-size: 0.85em; text-decoration: none;">
    Download PDF CV
  </a>
</div>

---

## Education

**Bachelor of Chemical Engineering with Research** <span style="float: right; color: #777;">2023 -- 2027</span>  
*Institute of Chemical Technology (ICT), Mumbai, India*  
* **Minor:** Artificial Intelligence & Machine Learning
* **Undergraduate Research Thesis:** *Valorization of \\(\text{CO}_2\\) to lower olefins via One-Pot Bifunctional Catalysis*  
  *Advisor: Prof. Ganapati D. Yadav, Emeritus Professor of Eminence & Former Vice Chancellor, ICT Mumbai*
* **CGPA:** 8.23 / 10

---

## Research Interests

Applied Mathematics, Process Systems Engineering, Fluid Mechanics, Scientific ML.

---

## Research Experience

**Indian Institute of Technology (IIT), Bombay** <span style="float: right; color: #777;">May 2026 -- Present</span>  
*Graph Decomposition for Fault-Tolerant Distributed MPC*  
*Advisor: Prof. Sujit Jogwar, Associate Professor, Department of Chemical Engineering, IIT Bombay*

* Developed a decomposition framework of integrated plants to synthesize robust control systems.
* Implemented an iterative Distributed MPC coordinator in Python (`CasADi`, `IPOPT`) on a benzene alkylation benchmark; controllers exchange full predicted input trajectories each coordination round.
* Cooperative DMPC came within 1.5% (3 controllers) and 0.1% (2 controllers) of centralized closed-loop cost, against a 22% gap for non-cooperative control. On sub-controller failure, reassigning its variables through overlapping memberships cut post-failure ISE about 7× compared with holding its inputs.

<br>

**Institute of Chemical Technology (ICT), Mumbai** <span style="float: right; color: #777;">Sept 2025 -- Present</span>  
*Adaptive Neural Surrogates & Inverse Optimization for Dynamical Systems*  
*Advisors: Dr. Shailesh Kumar, Chief Data Scientist, Centre of Excellence in AI/ML, Jio Platforms*  
*& Prof. Ajit Kumar, Professor & Head, Department of Mathematics, ICT Mumbai*

* Built neural network surrogate models of benchmark optimization functions using error-driven adaptive spatial sampling, applying quantile-based error flagging and PCA-aligned subgrids to resolve steep response gradients with minimal training points.
* Implemented Physics-Informed Neural Networks (PINNs) in PyTorch to recover unknown parameters from Navier--Stokes flow fields.
* Formulated a gradient-driven inverse optimization framework using Moore-Penrose pseudoinverse to optimize inputs directly on learned neural landscapes.

<br>

**Institute of Chemical Technology (ICT), Mumbai** <span style="float: right; color: #777;">Dec 2024 -- July 2025</span>  
*Multiphase CFD*  
*Advisor: Prof. Ashwin W. Patwardhan, Senior Professor, Department of Chemical Engineering, ICT Mumbai*

* Studied fluid mechanics, turbulence modeling and CFD for incompressible single- and multiphase systems, and performed numerical simulations using OpenFOAM.
* Simulated transient droplet hydrodynamics and interfacial dynamics for immiscible systems (toluene rising in water) using the Volume of Fluid (`interFoam`) method in OpenFOAM.

---

## Academic Achievements

* **GATE 2026 (Graduate Aptitude Test in Engineering):**
  * **AIR 76** in Chemical Engineering (CH), among 14,070 ChemE candidates across India.
  * **AIR 370** in Engineering Sciences (XE: Engineering Mathematics, Fluid Mechanics, Thermodynamics).
* **iGEM Competition 2025:** Team member (modelling); translated and implemented the Belov et al. DSB-repair model in Python. Team won Gold.

<br>

**Gas-Inducing Impeller Design Competition (1st Place)** <span style="float: right; color: #777;">Jan 2026</span>  
*Tinkerer's Lab, ICT Mumbai*
* Secured 1st place among 7 teams; designed and fabricated a 92 mm literature-derived gas-inducing impeller (CAD, 3D printing, laser cutting).
* Achieved gas induction onset at \\(N_{cg} = 368\text{ rpm}\\) and solid suspension (\\(d_p \approx 2\text{--}3\text{ mm}\\)) at \\(500\text{ rpm}\\), delivering a mass-transfer-to-power efficiency of \\(1.02 \times 10^{-5}\text{ mol L}^{-1}\text{ s}^{-1}\text{ W}^{-1}\\) (1.8× over runner-up) and an average \\(\text{CO}_2\\)--NaOH mass transfer rate of \\(1.26 \times 10^{-3}\text{ mol L}^{-1}\text{ s}^{-1}\\) at 123 W.

---

## Skills

* **Programming:** Python (`PyTorch`, `NumPy`, `SciPy`, `CasADi`), C++
* **Scientific Computing:** Numerical Methods, Neural Networks, Computational Fluid Dynamics, High Performance Computing
* **Tools:** OpenFOAM, Linux, Bash, Git, LaTeX, DWSIM, Simulink

---

## Selected Academic Projects

* **Seminar:** Literature review, *Hydrodynamics of Countercurrent Liquid--Liquid Contactors*; unified by local energy dissipation \\(\varepsilon\\): from Hinze scaling and population balances to recent DNS of drop breakup. Compiled and compared correlations for \\(d_{32}\\), holdup, slip velocity, flooding and axial dispersion across five column types.
* **Plant Design:** Designing a 20 TPA plant for vitamin B12 (cyanocobalamin) via aerobic fed-batch fermentation of *P. denitrificans*: market-based capacity sizing, route and microbe selection, and site selection. Completed fermentation kinetics, broth property estimation, PFD, and material & energy balances for a 38-fermenter staggered train.
* **Cryptography:** Designed and implemented an original cryptographic encoding-decoding scheme inspired by Hamming codes. Implemented RSA and Elliptic Curve Cryptography (ECC) in Python, exploring their number-theoretic and geometric foundations, the Discrete Logarithm Problem (DLP), and Diffie--Hellman and ElGamal key-exchange protocols.
* **Thermodynamics:** Built a Python program for TAC minimization vs. operating pressure for a binary distillation column, using NRTL-regressed VLE data.
* **Data Analytics & ML:** Performed exploratory data analysis and regression-based modelling of geospatial temperature data for climate trend analysis. Conducted statistical and spatial analysis of geochemical data using unsupervised learning techniques.
* **Optimization:** Implementation of direct search methods using SageMath. Implementation and benchmarking of the L-BFGS method in Python.

---

## Extra-Curricular Activities

**Codathon — Vortex 13.0 Tech Fest** <span style="float: right; color: #777;">Jan 2026</span>  
*ICT Mumbai*
* Designed the problem statement, coordinated evaluation and logistics for the coding competition.
