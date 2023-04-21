---
title: "Problems"
weight: 1
---

The problem is one on the basic TRUST classes (known as `Probleme_base` in the C++ code). It is impossible to run a TRUST computation without defining explicitly what problem you want to solve.

In short, the problem defines what equations you are willing to solve. Lets say that you want to solve a hydraulic problem, then the Navier-Stokes equation is only treated. Besides, if a user defines a thermal-hydraulic problem, then two equations will be considered: Navier-Stokes and a temperature convection/diffusion equation.

# TRUST Problems

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

The C++ class is `Pb_Multiphase`. This problem allows the resolution of N - phases via a model of 3N equations; where the equations solved for each phase are the momentum, mass and energy equations.

The coupling between all equations is done in a strong way; ie: a single matrix for all equations is used. This problem is available with all versions of the PolyMAC discretizations and with VDF. Calling EOS and [CoolProp](http://www.coolprop.org/) to compute the thermao-Physical Properties is possible for this problem via the `TPPI` interface.

## Coupled Problems

There are two ways to solve coupled problems with TRUST: either using `Probleme_Couple` or via the Interface of Code Coupling **([ICoCo](https://github.com/cea-trust-platform/icoco-coupling))** and `ProblemeTrio`.

In `Probleme_Couple`, the user has to define the contact boundary/volume between several domains where a TRUST problem (one of the above) is to be defined on each domain. The coupling is managed by TRUST where a point-fixed algorithm is used to converge the coupled simulation.

On the other hand, the user is to write his own coupling algorithm when using ICoCo. In practice, this can be done either by writing a C++ supervisor to manage the coupling, or via its python interface available in TRUST, thanks to the swig wrapper !


# Complete Problem Examples

Here a list of problem blocs taken from selected TRUST test cases. Have a look on these test cases available in `$TRUST_ROOT/tests`.

---
## Conduction

Bloc taken from TRUST's `docond_VEF` test case.

	# Read a conduction problem pb #
	Read pb 
	{
		# Read solid medium #
	    Solide
	    {
	        rho Champ_Uniforme 1 1000.
	        lambda Champ_Uniforme 1 250.
	        Cp Champ_Uniforme 1 100.
	    }
	    
		# Read conduction equation #
	    Conduction
	    {
	        diffusion { }
	        initial_conditions { temperature Champ_Uniforme 1 30. }
	        boundary_conditions 
	        {
	            paroi_a_40 paroi_temperature_imposee Champ_Front_Uniforme 1 40.
	            paroi_a_20 paroi_temperature_imposee Champ_Front_Uniforme 1 20.
	            Paroi_echange paroi_contact pb2 Paroi_echange
	        }
	    }
	    
	    # Read post-processing #
	    Post_processing
	    {
	        Probes
	        {
	            sonde_tsol temperature periode 1. points 2 0.15 0.55 0.55 0.15
	            sonde_segs temperature periode 5. segment 10 0. 0.75 0.3 0.75
	        }
	        fields dt_post 300.
	        {
	            temperature elem
	        }
	    }
	    
	    # Save the data at the end of the calculation to resume later #
	    sauvegarde formatte solide.rep
	}
	
---
## Thermal Hydraulic Problem

Bloc taken from TRUST's `Source_Generique_PolyMAC` test case.

	# Read a thermal hydraulic problem pb #
	Read pb
	{
		 # Read incompressible medium #
	    fluide_incompressible 
	    {
	        gravite champ_uniforme 2 0 -9.81
	        mu Champ_Uniforme 1 1.85e-5
	        rho Champ_Uniforme 1 1.17
	        lambda Champ_Uniforme 1 0.0262
	        Cp Champ_Uniforme 1 1006
	        beta_th Champ_Uniforme 1 3.41e-3
	    }
	
		# Read NS equation #
	    Navier_Stokes_standard
	    {
	        solveur_pression petsc gcp { precond null { } seuil 1e30 }
	        convection { negligeable }
	        diffusion { negligeable }
	        initial_conditions { vitesse Champ_Uniforme 2 0. 0. }
	        boundary_conditions 
	        {
	            Obstacle paroi_fixe
	            Symetrie symetrie
	            Sortie frontiere_ouverte_pression_imposee Champ_front_Uniforme 1 0.
	            Entree frontiere_ouverte_vitesse_imposee  Champ_front_Uniforme 2  1. 0.
	        }
	        sources 
	        {
	            Source_Qdm Champ_fonc_xyz dom 2 x y
	        }
	    }
		 
		 # Read Temperature equation #
	    Convection_Diffusion_Temperature
	    {
	        diffusion { negligeable }
	        convection { negligeable }
	        boundary_conditions
	        {
	            Symetrie 	paroi_adiabatique
	            Obstacle 	paroi_adiabatique
	            Entree  	frontiere_ouverte_temperature_imposee Champ_front_Fonc_xyz 1 1
	            Sortie	frontiere_ouverte_temperature_imposee Champ_front_Fonc_xyz 1 0
	        }
	        initial_conditions { Temperature Champ_Fonc_xyz dom 1 0 }
	        sources 
	        {
	            Puissance_Thermique champ_fonc_xyz dom 1 50+x+y
	        }
	    }
	
		 # Read post-processings #
	    Postraitement
	    {
	        fields dt_post 1e10
	        {
	            temperature elem
	            vitesse elem
	            pressure elem
	        }
	    }
	}
	
---
## Multi-Phase Problem

Bloc taken from TRUST's `CoolProp_water_BICUBIC_HEOS_with_sat` test case. Attention this test case needs a TRUST version linked with the CoolProp library.

	# Read a multiphase problem pb #
	Read pb
	{
		# Read the multi-phase + saturation medium #
		Milieu_composite
		{
			liquide_eau Fluide_generique_coolprop { model BICUBIC&heos fluid Water phase liquid } 
			gaz_eau Fluide_generique_coolprop { model BICUBIC&heos fluid Water }
			saturation_eau saturation_generique_coolprop { model BICUBIC&heos fluid Water phase liquid }
		}
 
		# Read the correlations #
		correlations
		{
		flux_interfacial coef_constant { liquide_eau 1.0e10 gaz_eau 1.0e10 }
		}
	    
	    # Read the momentum equation #
	    QDM_Multiphase
	    {   
	        evanescence { homogene { alpha_res 1 alpha_res_min 0.5 } } 
	        solveur_pression petsc cli_quiet { -pc_type hypre -pc_hypre_type boomeramg }
	        convection { amont }
	        diffusion  { negligeable } 
	        initial_conditions
	        {
	           vitesse  Champ_fonc_xyz dom 4 2.0*(x>0.5)-2.0*(x[0.5) 2.0*(x>0.5)-2.0*(x[0.5) 0.0 0.0
	           pression Champ_fonc_xyz dom 1 1.0e5 
	        }
	        boundary_conditions
	        {
	            haut symetrie
	            bas symetrie
	             gauche frontiere_ouverte_pression_imposee champ_front_uniforme 1 100000.0
	            droite frontiere_ouverte_pression_imposee champ_front_uniforme 1 100000.0
	        }
	    }
	    
	    # Read the mass equuation #
	    Masse_Multiphase
	    {
	        initial_conditions { alpha Champ_Fonc_xyz dom 2 0.95 0.05 }
	        convection { amont }
	        boundary_conditions
	        {
	            haut paroi
	            bas paroi
	            gauche frontiere_ouverte a_ext Champ_Front_Uniforme 2 0.95 0.05
	            droite frontiere_ouverte a_ext Champ_Front_Uniforme 2 0.95 0.05
	        }
	          sources { flux_interfacial }
	    }
	    
	    # Read the energy equation #
	    Energie_Multiphase
	    {
	        diffusion { negligeable }
	        convection { amont }
	        initial_conditions { temperature Champ_fonc_xyz dom 2 10. 10. }
	        boundary_conditions
	        {
	            haut paroi_adiabatique
	            bas paroi_adiabatique
	            gauche frontiere_ouverte T_ext Champ_Front_uniforme 2 81.578 61.578
	            droite frontiere_ouverte T_ext Champ_Front_uniforme 2 71.578 51.578
	        }
	         sources { flux_interfacial }
	    }
	    
	    # Read the post-processing #
	    Post_processing
	    {
	        probes
	        {
	             rho grav masse_volumique periode 1e8 segment 1000 0 0.5 1 0.5
	               v grav         vitesse periode 1e8 segment 1000 0 0.5 1 0.5
	               p grav        pression periode 1e8 segment 1000 0 0.5 1 0.5
	            eint grav energie_interne periode 1e8 segment 1000 0 0.5 1 0.5
	        }
	        
	        Format lml
	        fields dt_post 1e-4
	        {
	            alpha elem
	            vitesse elem
	            pression elem
	            temperature elem
	            energie_interne elem
	            vitesse_liquide_eau elem
	            vitesse_gaz_eau elem
	            alpha_gaz_eau elem
	            masse_volumique elem
	        }
	    }
	}