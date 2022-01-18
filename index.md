---
layout: default
---

Transition to turbulence is a long-standing problem in fluid dynamics with far reaching consequences, both from an academic and industrial point of view.
Numerous tools from dynamical system theory can be used to improve our understanding, yet, few are available for generic [CFD](https://en.wikipedia.org/wiki/Computational_fluid_dynamics) solvers.
**nekStab** is one of them.
Its aim is to extend the capabilities of [Nek5000](https://nek5000.mcs.anl.gov/), a spectral element solver well established in the academic hydrodynamic and aerodynamic community.
Leveraging Krylov-based techniques and Nek5000's high-performances capabilities, **nekStab** provides a set of algorithms to compute (stable and unstable) fixed points and periodic orbits of the Navier-Stokes equations as well as quantifying their stability through eigenvalue or singular value analysis.

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
