---
title: "Problems"
weight: 1
---

The problem is one on the basic TRUST classes (known as `Probleme_base` in the C++ code). It is impossible to run a TRUST computation without defining explicitly what problem you want to solve.

In short, the problem defines what equations you are willing to solve. Lets say that you want to solve a hydraulic problem, then the Navier-Stokes equation is only treated. Besides, if a user defines a thermal-hydraulic problem, then two equations will be considered: Navier-Stokes and a temperature convection/diffusion equation.

Here are all the problems available in TRUST.

- **Conduction Problem**

	The C++ class is `Pb_Conduction`. Here, only a heat equation is considered.

- **Hydraulic Problem**

	The C++ class is `Pb_Hydraulique`. Here, only the Navier-Stokes equation is considered.

- **Hydraulic Concentration Problem**

	The C++ class is `Pb_Hydraulique_Concentration`. Here, a Navier-Stokes and a passive scalar transport equations are considered.

- **Hydraulic Binary-Mixture LMN Problem**

	For this type of problem, two C++ classes are available: `Pb_Hydraulique_Melange_Binaire_QC` and `Pb_Hydraulique_Melange_Binaire_WC`. Such a problem can be used when dealing with binary mixture flows with an iso-thermal assumption. 
	
	Two equations are solved, the Navier-Stokes and a mass-fraction convection diffusion equation of the first species. The second mass fraction is deduced from the fact of the unity of the mass fractions. In these problems, all acostics effects are filtered out and the pressure is splitted into two pressures; the hydrodynamic (used in the Navier-Stokes equation) and the thermodynamic pressure (used in the state equation).
	
	The difference between the two problems is that `Pb_Hydraulique_Melange_Binaire_QC` considers an iso-bar assumption (thermodynamic pressure is uniform in space). Besides, this is not the case for `Pb_Hydraulique_Melange_Binaire_WC` (Weakly-Compressible assumption) where the total pressure is used in equation of states. This allows the pressure drop, or the hydro-static pressure, to influence the density variation.

- **Thermal Hydraulic Problem**

	The C++ class is `Pb_Thermohydraulique`. Here, the Navier-Stokes and the temperature convection-diffusion equations are considered.

- **Thermal Hydraulic Concentration Problem**

	The C++ class is `Pb_Thermohydraulique_Concentration`. Here, three equations are treated: the Navier-Stokes, the temperature convection-diffusion and a passive scalar transport equation.

- **Thermal Hydraulic LMN Problem**

	For this type of problem, two C++ classes are available: `Pb_Thermohydraulique_QC ` and `Pb_Thermohydraulique_WC `. The same assumptions are considered for both problems as those considered previously in the binary-mixture problems. However, here the equations solved are the Navier-Stokes and the temperature, which means that the problem is not iso-thermal.

- **Thermal Hydraulic Multi-Species LMN Problem**


- **Coupled Problem**


- **Multi-Phase Problem**

