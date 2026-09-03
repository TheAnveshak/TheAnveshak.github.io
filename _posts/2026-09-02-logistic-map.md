---
title: "Discrete Dynamics & Cobweb Maps in Julia"
date: 2026-09-02
permalink: /posts/2026/09/logistic-map/
tags:
  - dynamical-systems
  - julia
  - scientific-computing
comments: true
excerpt: "Building a reactive Pluto.jl notebook to visualize step-by-step cobweb iterations and period-doubling in discrete maps."
---

This notebook came out of the discrete dynamical systems module in Prof. Amiya Bhowmick's *Mathematical Modelling* course at ICT Mumbai, which covers 1D maps, stability analysis, and bifurcation theory.

I built this notebook to make the iteration geometry explicit. It renders the cobweb mapping for the logistic map \\( x_{n+1} = r x_n (1 - x_n) \\) across parameter sweeps, tracking how stability shifts at fixed points and cascades into multi-cycles. 

I wrote this in Julia using Pluto.jl rather than standard Jupyter widgets because Pluto manages cell reactivity through a dependency graph. Changing the growth parameter \\( r \\) or the initial state \\( x_0 \\) updates the recurrence relations and re-renders the trajectory deterministically, without hidden notebook state or cell execution order bugs.

The embedded reactive notebook is below. Everything—from the iteration loops to the interactive sliders—runs directly inside it.
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/) *(Click above to launch the live reactive Julia kernel on MyBinder. First build may take a few minutes).*

<div style="width: 100%; height: 85vh; min-height: 700px; margin-top: 1.5rem; border-radius: 8px; overflow: hidden; border: 1px solid #30363d;">
  <iframe 
    src="https://pluto.land/n/32py5bjh" 
    style="width: 100%; height: 100%; border: none;"
    loading="lazy"
    allow="clipboard-read; clipboard-write"
    sandbox="allow-forms allow-modals allow-popups allow-same-origin allow-scripts">
  </iframe>
</div>

---
