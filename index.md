---
layout: default
---

|:---:|:---:|:---:|:---:|
|[Home](./)|[News]()|[Showcase]()|[Documentation]()|

Transition to turbulence is a long-standing problem in fluid dynamics, pioneered by [Osbourne Reynolds](https://en.wikipedia.org/wiki/Osborne_Reynolds) as early as 1883.
Today, numerous tools from dynamical system theory can be used to improve our understanding of this process, yet, few libraries are available for generic [CFD](https://en.wikipedia.org/wiki/Computational_fluid_dynamics) solvers.
**nekStab** is one of them.
Its aim is to extend the capabilities of [Nek5000](https://nek5000.mcs.anl.gov/), a well established spectral element solver in the academic hydrodynamic and aerodynamic community.
Leveraging Krylov-based techniques and Nek5000's high-performance time-stepping capabilities, **nekStab** provides a set of algorithms to compute stable and unstable fixed points or periodic orbits of the Navier-Stokes equations as well as quantifying their stability properties through eigenvalue or singular value analysis.

# Getting started

**nekStab** is a toolbox written in `Fortran 90` for the spectral element solver Nek500.
Both of them can be installed on Ubuntu/Debian distros using the following commands.

**Prerequisites**

```bash
sudo apt-get -y install libmpich-dev libopenblas-dev cmake m4
```

**Cloning the repository**

```bash
git clone --depth=50 --branch=master https://github.com/ricardofrantz/nekStab.git
cd nekStab
git submodule update --init --recursive
```

Run `vim $HOME/.bashrc` and add the following :

```bash
export NEKSTAB_SOURCE_ROOT=$HOME/nekStab
export NEK_SOURCE_ROOT=$NEKSTAB_SOURCE_ROOT/Nek5000
export PATH=$NEK_SOURCE_ROOT/bin:$PATH
```

Computing the fixed point for the cylinder flow example using the Newton-Krylov solver on 4 processors is as simple as

```bash
cd ~/nekStab/examples/cylinder/1_2baseflow/newton
./cmpile.sh all
nekbmpi 1cyl 4
```

More information about compiling the code on Mac OS or optional packages is available in the [**Documentation**](https://ricardofrantz.github.io/nekStabDoc/en/master/index.html).

# Core components

- **Krylov subspace methods :** At its core **nekStab** relies on [Krylov subspace methods](https://en.wikipedia.org/wiki/Krylov_subspace) for most large-scale linear algebra tasks.
The most important one is the [Arnoldi iteration](https://en.wikipedia.org/wiki/Arnoldi_iteration) on top of which are build a [GMRES](https://en.wikipedia.org/wiki/Generalized_minimal_residual_method) solver for linear systems of equations, a [Krylov-Schur](404) method for eigenvalue analysis and [Lanczos](https://en.wikipedia.org/wiki/Lanczos_algorithm) algorithm for singular value decomposition of the linearized Navier-Stokes operator.
- **Fixed points and periodic orbits :**
- **Eigenvalue and singular value analysis :**
