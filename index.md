---
title: TRUST Platform
layout: home
description: TRUST - The CEA's open source HPC CFD platform
intro_image: "images/illustrations/TRUST.png"
intro_image_absolute: false
intro_image_hide_on_mobile: true
---

# TRUST - The CEA's open source HPC platform

**TRUST** is a High Performance Computing (HPC) thermohydraulic engine for Computational Fluid Dynamics (CFD) developed at the Departement of System and Structure Modelisation (DM2S) of the French Atomic Energy Commission (CEA).

The acronym **TRUST** stands for **TR**io**U** **S**oftware for **T**hermohydraulics. This software was originally designed for conduction, incompressible single-phase, and Low Mach Number (LMN) flows with a robust Weakly-Compressible (WC) multi-species solver. 

However, a huge effort has been conducted recently, and now **TRUST** is able to simulate real compressible multi-phase flows. 

**TRUST** is also progressively ported to support GPU acceleration, using the [Kokkos](https://kokkos.org/kokkos-core-wiki/) library. It has been selected to be a demonstrator of the [CExA](https://cexa-project.org/) project.  

**TRUST** aslo serves as the kernel of several CEA's application codes. The [TrioCFD](https://triocfd.cea.fr/) software is an open source example of such codes. The main application of TrioCFD is turbulence modelling.

This software is OpenSource (**[BSD license](https://github.com/cea-trust-platform/trust-code/blob/master/License.txt)**), available **[here](https://github.com/cea-trust-platform/trust-code)** on GitHub. 

**TRUST** also relies on the following other open source products: [SALOME](https://www.salome-platform.org/?lang=fr), [MEDCoupling](https://github.com/SalomePlatform/medcoupling), [ICoCo](https://github.com/cea-trust-platform/icoco-coupling), [VisIt](https://visit-dav.github.io/visit-website/index.html), ...
