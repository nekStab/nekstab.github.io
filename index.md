---
layout: default
---

Transition to turbulence is a long-standing problem in fluid dynamics with far reaching consequences, both from an academic and industrial point of view.
Numerous tools from dynamical system theory can be used to improve our understanding, yet, few are available for generic [CFD](https://en.wikipedia.org/wiki/Computational_fluid_dynamics) solvers.
**nekStab** is one of them.
Its aim is to extend the capabilities of [Nek5000](https://nek5000.mcs.anl.gov/), a spectral element solver well established in the academic hydrodynamic and aerodynamic community.
Leveraging Krylov-based techniques and Nek5000's high-performances capabilities, **nekStab** provides a set of algorithms to compute (stable and unstable) fixed points and periodic orbits of the Navier-Stokes equations as well as quantifying their stability through eigenvalue or singular value analysis.

