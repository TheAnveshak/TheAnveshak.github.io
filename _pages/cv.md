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
  <span><strong>Arya Rane</strong> — Academic & Research Record</span>
  <a href="{{ base_path }}/files/AryaRane_3_5.pdf" class="btn btn--primary" target="_blank" style="margin: 0; padding: 0.4em 1em; font-size: 0.85em; text-decoration: none;">
    Download PDF CV
  </a>
</div>

---

## Education

**Bachelor of Chemical Engineering** <span style="float: right; color: #777;">2023 – 2027</span>  
*Institute of Chemical Technology (ICT), Mumbai*  
* **Minor:** Artificial Intelligence & Machine Learning
* **CGPA:** 8.23 / 10
* **Undergraduate Thesis:** *Valorization of \\(\text{CO}_2\\) to lower olefins via One-Pot Bifunctional Catalysis*  
  *Advisor: Prof. G. D. Yadav*

---

## Research Experience

**Indian Institute of Technology (IIT), Bombay** <span style="float: right; color: #777;">May 2026 – Present</span>  
*Overlapping Graph Decomposition for Fault-Tolerant Distributed MPC*  
*Advisor: Prof. Sujit Jogwar*

* Formulated an overlapping network decomposition scheme for integrated process plants via Genetic Algorithm on weighted equation graphs.
* Implemented an iterative Distributed MPC coordinator in Python using `do_mpc` and `IPOPT`, resolving coupled nonlinear subproblems via trajectory broadcasting and input-freezing protocols.
* Validated dynamic fault tolerance on a multiple-recycle benzene alkylation benchmark, achieving zero tracking degradation (identical ISE) with a 30.8% solve-time reduction under sub-controller failure.

<br>

**Institute of Chemical Technology (ICT), Mumbai** <span style="float: right; color: #777;">Sept 2025 – Present</span>  
*Adaptive Neural Surrogates & Inverse Optimization for Dynamical Systems*  
*Advisors: Prof. Ajit Kumar & Dr. Shailesh Kumar*

* Built neural network surrogate models using error-driven adaptive spatial sampling, applying quantile-based error flagging and PCA-aligned subgrids to resolve steep response gradients with minimal training points.
* Implemented Physics-Informed Neural Networks (PINNs) in PyTorch to solve forward dynamics and inverse parameter discovery (NSE flow fields) via automatic differentiation of differential residuals.
* Formulated a gradient-driven inverse optimization framework utilizing Moore-Penrose pseudoinverse to optimize inputs directly on learned neural landscapes.

<br>

**Institute of Chemical Technology (ICT), Mumbai** <span style="float: right; color: #777;">Dec 2024 – July 2025</span>  
*Multiphase CFD & Interfacial Hydrodynamics*  
*Advisor: Prof. Ashwin W. Patwardhan*

* Studied fluid mechanics, turbulence modeling, and CFD for incompressible single- and multiphase systems, performing numerical simulations in OpenFOAM.
* Simulated transient bubble hydrodynamics and interfacial dynamics for immiscible systems (benzene/toluene rising in water) using the Volume of Fluid (`interFoam`) method.

---

## Academic Achievements

* **GATE 2026 (Graduate Aptitude Test in Engineering):**
  * **AIR 76** in Chemical Engineering (CH) out of 14,070 candidates.
  * **AIR 370** in Engineering Sciences (XE: Engineering Mathematics, Fluid Mechanics, Thermodynamics).
* **iGEM Competition 2025 (Gold Medal):** Co-developed DNA-damage repair (Belov model) model translation and algorithmic execution.

---

## Technical Skills

* **Programming:** Python (`PyTorch`, `NumPy`, `SciPy`, `CasADi`), C++
* **Scientific Computing:** Numerical Methods, Dynamic Optimization, Neural Networks, PINNs, CFD, HPC Workflows
* **Engineering Tools:** OpenFOAM, Linux, Bash, Git, LaTeX, DWSIM, Simulink

---

## Selected Technical Projects

* **Thermodynamic Optimization:** Developed a Python pipeline for Total Annualized Cost (TAC) minimization versus operating pressure for a binary distillation column using NRTL-regressed VLE data.
* **Optimization Benchmarking:** Implemented direct search routines in SageMath, evaluated Sequential Quadratic Programming (SQP) variants, and benchmarked L-BFGS optimization implementations in Python.
* **Data Analytics & Geospatial ML:** Performed exploratory data analysis and regression modeling on geospatial temperature datasets for climate trend analysis, alongside unsupervised spatial analysis of geochemical data.

---

## Leadership & Competitions

**Gas-Inducing Impeller Design Competition (1st Place)** <span style="float: right; color: #777;">Jan 2026</span>  
*Tinkerer's Lab, ICT Mumbai*
* Designed and fabricated a 92 mm hollow-shaft gas-inducing impeller (CAD, 3D printing, laser cutting).
* Achieved critical gas induction at \\(N_{cg} = 368\text{ RPM}\\) and solid suspension at \\(500\text{ RPM}\\), delivering a mass-transfer-to-power efficiency of \\(1.02 \times 10^{-5}\text{ mol L}^{-1}\text{ s}^{-1}\text{ W}^{-1}\\).

<br>

**Codathon — Vortex 13.0 Tech Fest** <span style="float: right; color: #777;">Jan 2026</span>  
*Lead Coordinator & Problem Setter*
* Formulated algorithmic problem statements, edge-case test suites, and automated evaluation sandboxes for the annual coding competition.
