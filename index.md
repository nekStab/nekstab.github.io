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

### News

- **February 2022:** The first release of `nekStab` is available online!
Its main modules are

    - [x] a Newton-Krylov solver for the computation of fixed points and periodic orbits.
    - [x] Eigensolver for the computation of the leading eigenpairs of the linearized Navier-Stokes operator for fixed points, and leading Floquet multipliers of  the monodromy matrix for periodic orbits.
    - [x] Singular value solver for the computation of the leading singular triplets of the linearized Navier-Stokes operator.
    - [x] Sensitivity analysis to baseflow modification or steady forcing of the eigenvalues.

---

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

- **Time-stepping strategy :** The aim of **nekStab** is to extend the capabilities of the Nek5000 solver to conduct large-scale bifurcation analysis.
To do so, a *time-stepping strategy* is employed to massively reduce the memory footprint of such calculations at the expense of an increasing, yet affordable, computational cost.
For more details about such a strategy, please refer to the original paper by [Edwards et al. (1994)](https://www.sciencedirect.com/science/article/pii/S0021999184710072) or the review by [Loiseau et al. (2019)](https://arxiv.org/pdf/1804.03859.pdf).
- **Krylov subspace methods :** At its core **nekStab** relies on [Krylov subspace methods](https://en.wikipedia.org/wiki/Krylov_subspace) for most large-scale linear algebra tasks.
The most important one is the [Arnoldi iteration](https://en.wikipedia.org/wiki/Arnoldi_iteration) on top of which are build a [GMRES](https://en.wikipedia.org/wiki/Generalized_minimal_residual_method) solver for linear systems of equations, a [Krylov-Schur](404) method for eigenvalue analysis and [Lanczos](https://en.wikipedia.org/wiki/Lanczos_algorithm) algorithm for singular value decomposition of the linearized Navier-Stokes operator.
- **Fixed points and periodic orbits :** Several algorithms are included to compute stable and unstable fixed points or periodic orbits of the Navier-Stokes equations, the most efficient one being a [Newton-Krylov](https://en.wikipedia.org/wiki/Newton%E2%80%93Krylov_method) solver.
The *selective frequency damping* method proposed by [Akervik et al. (2006)](https://www.mech.kth.se/~luca/papers/SFD_PoF.pdf) and *BoostConv* by [Citro et al. (2017)](https://www.sciencedirect.com/science/article/pii/S0021999117303698?casa_token=XtEKhWIYrQAAAAAA:PE_AnCzzNSRfbclSvA5NEtxUsBgQTwh38bH0NG3a9l1NRaDMFYnYqbsZb9Wb_ItjlynZHX7cavEK) are also proposed to compute fixed points.
For periodic orbits, additional algorithms include *Dynamic Mode Tracking* by [Queguineur et al. (2019)](https://aip.scitation.org/doi/abs/10.1063/1.5085474) and *Time-delayed Feedback* by [Shaabani-Ardali et al. (2017)](https://journals.aps.org/prfluids/abstract/10.1103/PhysRevFluids.2.113904).
- **Eigenvalue and singular value analysis :** Asymptotic and short-time instabilities are governed by the eigenvalues and singular values, respectively, of the linearized Navier-Stokes operator.
In **nekStab**, these are computed using a straightforward implementation of the Krylov-Schur solver originally proposed by [Stewart (2002)](https://epubs.siam.org/doi/10.1137/S0895479800371529).

# Citation

If you use **nekStab**, please consider citing one of the following papers :
- [Loiseau et al. (2019)](https://arxiv.org/pdf/1804.03859.pdf) presents most of the theoretical framework underlying **nekStab**.
```bibtex
@incollection{chapter:loiseau:2019,
  title={Time-stepping and Krylov methods for large-scale instability problems},
  author={Loiseau, J.-Ch. and Bucci, M. A. and Cherubini, S. and Robinet, J.-Ch.},
  booktitle={Computational Modelling of Bifurcations and Instabilities in Fluid Dynamics},
  pages={33--73},
  year={2019},
  publisher={Springer}
}
```
- [Loiseau et al. (*J. Fluid Mech.*, 2014)](https://sam.ensam.eu/bitstream/handle/10985/8974/DYNFLUID-JFM-LOISEAU-2014.pdf?sequence=1&isAllowed=y) describes the first implementation of the Arnoldi solver in Nek5000.
```bibtex
@article{jfm:loiseau:2014,
    title={Investigation of the roughness-induced transition: global stability analyses and direct numerical simulations},
    author={Loiseau, J.-Ch. and Robinet, J.-Ch. and Cherubini, S. and Leriche, E.},
    journal={J. Fluid Mech.},
    volume={760},
    pages={175--211},
    year={2014},
}
```
