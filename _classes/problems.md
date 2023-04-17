---
title: "Problems"
weight: 1
---

The problem is one on the basic TRUST classes (known as `Probleme_base` in the C++ code). It is impossible to run a TRUST computation without defining explicitly what problem you want to solve.

In short, the problem defines what equations you are willing to solve. Lets say that you want to solve a hydraulic problem, then the Navier-Stokes equation is only treated. Besides, if a user defines a thermal-hydraulic problem, then two equations will be considered: Navier-Stokes and a temperature convection/diffusion equation.

Here are all the problems available in TRUST.

## Conduction Problem

The C++ class is `Pb_Conduction`. The heat equation is only considered in this problem.

## Hydraulic Problems

- **Hydraulic Problem**

	The C++ class is `Pb_Hydraulique`. Here, only the Navier-Stokes equation is considered.

- **Hydraulic Binary-Mixture LMN Problem**

	For this type of problem, two C++ classes are available: `Pb_Hydraulique_Melange_Binaire_QC` and `Pb_Hydraulique_Melange_Binaire_WC`. Such a problem can be used when dealing with binary mixture flows with an iso-thermal assumption. LMN stands for Low Mach Number flows.
	
	Two equations are solved, a Navier-Stokes and a mass-fraction convection diffusion equation for the first species. The second mass fraction is deduced from the fact of the unity of the sum of the mass fractions. In these problems, all acostics effects are filtered out and the pressure is splitted into two pressures; the hydrodynamic (time-space varying and is used in the Navier-Stokes equation) and the thermodynamic pressure (used in the state equation).
	
	The main difference between the two problems is that `Pb_Hydraulique_Melange_Binaire_QC` considers an iso-bar assumption (thermodynamic pressure is uniform in space). This is not the case for `Pb_Hydraulique_Melange_Binaire_WC` (Weakly-Compressible assumption) where the total pressure is used in equation of state. This allows the pressure drop and/or the hydro-static pressure to influence the density variation.
	
- **Hydraulic Concentration Problem**

	The C++ class is `Pb_Hydraulique_Concentration`. The equations considered in this problem are the Navier-Stokes and the convection/diffusion constituan equations.
	
- **Hydraulic Concentration Passive-Scalar Problem**

	The C++ class is `Pb_Hydraulique_Concentration_Scalaires_Passifs`. The equations considered in this problem are the Navier-Stokes, the convection/diffusion constituant and a list of additional passive-scalar equations.


## Thermal Hydraulic Problems

- **Thermal Hydraulic Problem**

	The C++ class is `Pb_Thermohydraulique`. Here, the Navier-Stokes and the temperature convection-diffusion equations are considered.
	
- **Thermal Hydraulic LMN Problem**

	For this type of problem, two C++ classes are available: `Pb_Thermohydraulique_QC ` and `Pb_Thermohydraulique_WC `. The same logic remains valid for both problems as that considered previously in the binary-mixture problems. However, the equations solved in these cases are the Navier-Stokes and the temperature equation, which means that the problem is not iso-thermal.
	
- **Thermal Hydraulic Multi-Species LMN Problem**

	The two C++ class available to deal with a problem of N-Species are `Pb_Thermohydraulique_Especes_QC` and `Pb_Thermohydraulique_Especes_WC`. Here, TRUST solves the following equations. 
	
	For `Pb_Thermohydraulique_Especes_QC`, the Navier-Stokes and temperature convection-diffusion equations are solved together with N additional mass-fractions equations. Besides, in `Pb_Thermohydraulique_Especes_WC`, the Navier-Stokes and temperature convection-diffusion equations are solved together with N-1 additional mass-fractions equations. The last mass fraction of species N is deduced from the fact of the unity of the sum of the N mass fractions. 

- **Thermal Hydraulic Concentration Problem**

	The C++ class is `Pb_Thermohydraulique_Concentration`. Here, three equations are treated: the Navier-Stokes, the temperature convection-diffusion and a convection/diffusion constituant equation.
	
- **Thermal Hydraulic Passive-Scalar Problem**

	The C++ class is `Pb_Thermohydraulique_Scalaires_Passifs`. Here, TRUST solves the following equations: the Navier-Stokes, the temperature convection-diffusion and a list of additional passive-scalar equations.
		
- **Thermal Hydraulic Concentration Passive-Scalar Problem**

	The C++ class is `Pb_Thermohydraulique_Concentration_Scalaires_Passifs`. Here, the treated equations are the following: the Navier-Stokes, the temperature convection-diffusion, the convection/diffusion constituant equation and a list of additional passive-scalar equations.

## Multi-Phase Problem

The C++ class is `Pb_Multiphase`. The problem allows the resolution of N - phases via a model of 3N equations. The coupling between all equations is done in a strong way; ie: a single matrix for all equations is used. This problem is available with all versions of the PolyMAC discretizations and with VDF. Calling EOS and [CoolProp](http://www.coolprop.org/) to compute the thermao-Physical Properties is possible for this problem via the `TPPI` interface.

## Coupled Problems

There are two ways to solve coupled problems with TRUST: either using `Probleme_Couple` or via the Interface of Code Coupling **([ICoCo](https://github.com/cea-trust-platform/icoco-coupling))** and `ProblemeTrio`.

In `Probleme_Couple`, the user has to define the contact boundary/volume between several domains where a TRUST problem (one of the above) is to be defined on each domain. The coupling is managed by TRUST where a point-fixed algorithm is used to converge the coupled simulation.

On the other hand, the user is to write his own coupling algorithm when using ICoCo. In practice, this can be done either by writing a C++ supervisor to manage the coupling, or via its python interface available in TRUST, thanks to the swig wrapper !
