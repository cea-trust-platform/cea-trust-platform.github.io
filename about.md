---
title: About
layout: page
description: About
---

TRUST is an object-oriented open-source platform written in C++, with approximately 230K lines of code and about 2500 classes. The code is managed on Git and built via CMake. Thanks to the Message Passing Interface (MPI) approach, the code is massively parallel. TRUST uses Doxygen to automatically generate HTML documentation from its source code.

The CFD platform has already been used to simulate a 1 m3 fuel cell hydrogen leakage scenario with 2 billion mesh elements using the Direct Numerical Simulation approach (DNS). The simulation was carried out on the IRENE-ROME cluster with 50k MPI processors.

Recently, TRUST has been ported to support GPU acceleration (NVidia/AMD).

TRUST has recently been ported to run on Linux distributions and MacOS. Support for Windows is currently in progress.

The platform is extensively tested (every day) on almost all distributions. The test database consists of about 1300 test cases (executed via ctest) and about 120 validation forms (executed via jupyter-notebook).

Using the TRUST post-processing format, it is possible to animate your simulations with a very nice particle visualization. This is done by the Snorky's project (**page under construction**) and can give something like that (click on the image to see the video on the youtube channel).

<div align="center">
  <a href="https://youtu.be/79mzSkn6yJk"><img src="https://github.com/cea-trust-platform/cea-trust-platform.github.io/blob/master/images/logo/logo-snorky.png?raw=true" alt="TRUST LOGO SNORKY" width="500px"></a>
</div>

# Historical background

The acronym TRUST stands for TRioU Software for Thermohydraulics. The platform was born on June 2015 after splitting Trio_U software (version 1.7.1) in two parts: TRUST & **[TrioCFD](https://triocfd.cea.fr/)**. Both parts are now open source and available **[here](https://github.com/cea-trust-platform)** on GitHub. 

Here are the main stages of the project's evolution, starting from the beginning of the Trio_U project:

- **1994 :** Start of the project Trio_U

- **1997 :** v1.0 - Finite Difference Volume (VDF) method only

- **1998 :** v1.1 - Finite Element Volume (VEF) method only

- **2000 :** v1.2 - Parallel MPI version

- **2001 :** v1.3 - Radiation model (TrioCFD now)

- **2002 :** v1.4 - LES turbulence models (TrioCFD now)

- **2006 :** v1.5 - VDF/VEF Front Tracking method (TrioCFD now)
 
- **2009 :** v1.6 - Data structure revamped

- **2015 :** v1.7 - Separation TRUST & TrioCFD + switch to open source

- **2019 :** v1.8 - New polyheadral discretization (PolyMAC)

- **2021 :** v1.8.4 - Multiphase problem + Weakly Compressible model

- **2022 ... :** Modern C++ code (templates, CRTP, ...), remove MACROS, GPU, ...

# Use and application domains

TRUST is capable of simulating laminar flows without any turbulence modeling, and it can also be used to simulate turbulent flows using a DNS approach. However, if a user intends to simulate a turbulent flow with a specific turbulence model, TrioCFD is required.

TRUST serves as a kernel for several independent private projects. As all the base classes and numerical methods are available in TRUST, building an application for a specific domain becomes easier by basing the project on TRUST. This is known as Build an Application Linked to Trio_U Kernel (BALTIK).

TRUST is now the foundation for several applications related to nuclear studies and safety, neutronics, porous media, batteries, fuel cells, and more.

# How to compile TRUST ?

In order to clone and compile the TRUST platform via a git configuration, open a bash terminal and simply execute the commands available in this **[README](https://github.com/cea-trust-platform/trust-code#readme)**.
