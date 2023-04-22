---
title: "Problems"
weight: 1
---

The problem is one on the basic TRUST classes (known as `Probleme_base` in the C++ code). It is impossible to run a TRUST computation without defining explicitly what problem you want to solve. 

Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classProbleme__base.html)** to see the Doxygen documentation of the `Probleme_base` class.

Inside the problem bloc, the user should define firstly the medium he wants to consider. All TRUST media derive from the class `Milieu_base`. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classMilieu__base.html)** to see the Doxygen documentation of this class.

**Attention:** The medium specified in the bloc should be coherent with the instantiated problem otherwise TRUST will throw an error. For example, defining an incompressible medium in a conduction problem is not possible because it makes no sense.

After defining the medium, the user should precise the equations he is willing to solve. All implemented equations derive from th ebase class `Equation_base`. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classEquation__base.html)** to see the Doxygen documentation of this class.

**Attention:** The equation should be also coherent with the instantiated problem otherwise TRUST will throw an error. For example, defining a Navier-Stokes equation in a conduction problem is not possible because it makes no sense.

Once all is defined, the user can define his post-processing flields and probes at the end of the problem bloc. This is optional and it is not a mandatory requirement that TRUST needs to solve your problem. Your are invited to read the Post-Processing page for a detailed description of the syntax and the available classes.

**Important:** You should explicitly tell TRUST to perform the simulation. This is done by the calling the C++ class `Solve` (or its alias `Resoudre`). Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classResoudre.html)** to see the Doxygen documentation of this class. If this is abscent from your data file, the problem with its media, equations and post-processings will be read by TRUST but not solved.  

**Conclusion:** The problem bloc defines the medium (media) you want to consider, the equations you are willing to solve and the post-processing (optional). Lets say that you want to solve a hydraulic problem, then an incompressible medium should be defined and the Navier-Stokes equation will be only treated. Besides, if a user defines a thermal-hydraulic problem, the same medium can be used but two equations will be considered in this case: the Navier-Stokes and the temperature convection/diffusion equations.

Here is a short summary of all the problems available in TRUST platform.

# Conduction Problem

The C++ class is `Pb_Conduction`. The heat equation is only considered in this problem. The medium and the equation should be respectively `Solide` and `Conduction`.

# Hydraulic Problems

In all cases, the medium should be `Fluide_Incompressible` or `Fluide_Ostwald`. An additional `Constituant` medium should be **secondly** defined if you ask for a problem with concentration.

- **Hydraulic Problem**

	The C++ class is `Pb_Hydraulique`. Here, only the Navier-Stokes equation is considered (equation `Navier_Stokes_standard`). 

- **Hydraulic Concentration Problem**

	The C++ class is `Pb_Hydraulique_Concentration`. The equations considered in this problem are the Navier-Stokes and the convection/diffusion constituant equations (respectively `Navier_Stokes_standard` and `Convection_Diffusion_Concentration`).
	
- **Hydraulic Concentration Passive-Scalar Problem**

	The C++ class is `Pb_Hydraulique_Concentration_Scalaires_Passifs`. The equations considered in this problem are the Navier-Stokes, the convection/diffusion constituant and a list of additional passive-scalar equations (respectively `Navier_Stokes_standard`, `Convection_Diffusion_Concentration` and a list of additional `Convection_Diffusion_Concentration` equations).


# Thermal Hydraulic Problems

In all cases, the medium should be `Fluide_Incompressible` or `Fluide_Ostwald`. An additional `Constituant` medium should be **secondly** defined if you ask for a problem with concentration.

- **Thermal Hydraulic Problem**

	The C++ class is `Pb_Thermohydraulique`. Here, the Navier-Stokes and the temperature convection-diffusion equations are considered (respectively `Navier_Stokes_standard` and `Convection_Diffusion_Temperature`).

- **Thermal Hydraulic Concentration Problem**

	The C++ class is `Pb_Thermohydraulique_Concentration`. Here, three equations are treated: the Navier-Stokes, the temperature convection-diffusion and a convection/diffusion constituant equation (respectively `Navier_Stokes_standard`,  `Convection_Diffusion_Temperature` and `Convection_Diffusion_Concentration`).
	
- **Thermal Hydraulic Passive-Scalar Problem**

	The C++ class is `Pb_Thermohydraulique_Scalaires_Passifs`. Here, TRUST solves the following equations: the Navier-Stokes, the temperature convection-diffusion and a list of additional passive-scalar equations (respectively `Navier_Stokes_standard`, `Convection_Diffusion_Temperature` and a list of additional `Convection_Diffusion_Concentration` equations).
		
- **Thermal Hydraulic Concentration Passive-Scalar Problem**

	The C++ class is `Pb_Thermohydraulique_Concentration_Scalaires_Passifs`. Here, the treated equations are the following: the Navier-Stokes, the temperature convection-diffusion, the convection/diffusion constituant equation and a list of additional passive-scalar equations (respectively `Navier_Stokes_standard`, `Convection_Diffusion_Temperature`, `Convection_Diffusion_Concentration` and a list of additional `Convection_Diffusion_Concentration` equations).

# Low Mach Number Problems

The Mach number Ma = u / c measures the ratio of the bulk velocity to the local speed of sound. Typically for Ma < 0.1, the compressibility effects can be neglected and the physical problem may be reduced by using a Low Mach Number (LMN) formulation. In such cases, the aspect of the problem orients towards simulating a mass variation rather than capturing and solving for the acoustic waves. This is basically the main hypothesis of a LMN approximation where all acoustic waves are filtered out.

Following a single scale asymptotic analysis referring to the Mach number, the zero Mach order equations are considered where the total pressure is decomposed into a thermodynamic pressure and a hydrodynamic pressure that fluctuatesin an order of Ma2. Note that the hydrodynamic pressure is much smaller than the thermodynamic one. The hydrodynamic pressure alone is used in the momentum equation, while the thermodynamic is used in the equation of state.

TRUST offers two LMN modelisations; either a Weakly-Compressible (WC) or a Quasi-Compressible (QC) models. The main difference between both models is that the QC model considers a space-uniform thermodynamic pressure. This is not the case in th WC model where the total pressure, which is space/time varying, is used in the equation of state. The last can have a great impact in some situations as the pressure drop and/or the hydro-static pressure can influence significantly the density variation.

Here are the available LMN problems in the TRUST platform. In all cases, either the `Fluide_Weakly_Compressible` or the `Fluide_Quasi_Compressible` medium **must** be used.

- **Hydraulic Binary-Mixture LMN Problem**

	For this type of problem, two C++ classes are available: `Pb_Hydraulique_Melange_Binaire_QC` and `Pb_Hydraulique_Melange_Binaire_WC`. Such a problem can be used when dealing with binary mixture flows with an iso-thermal assumption.
	
	Two equations are solved, a Navier-Stokes and a mass-fraction convection diffusion equation for the first species; either `Navier_Stokes_WC` and `Convection_Diffusion_Espece_Binaire_WC `, or `Navier_Stokes_QC ` and `Convection_Diffusion_Espece_Binaire_QC`. The mass fraction of the second species is deduced from the fact of the unity of the sum of the mass fractions. .
	
- **Thermal Hydraulic LMN Problem**

	For this type of problem, two C++ classes are available: `Pb_Thermohydraulique_QC ` and `Pb_Thermohydraulique_WC `. The equations solved in these cases are the Navier-Stokes and the temperature equation, which means that the problem is not iso-thermal (either `Navier_Stokes_WC` and `Convection_Diffusion_Chaleur_WC `, or `Navier_Stokes_QC ` and `Convection_Diffusion_Chaleur_QC`).
	
- **Thermal Hydraulic Multi-Species LMN Problem**

	The two C++ class available to deal with a problem of N-Species are `Pb_Thermohydraulique_Especes_QC` and `Pb_Thermohydraulique_Especes_WC`. Here, TRUST solves the following equations. 
	
	For `Pb_Thermohydraulique_Especes_QC`, the Navier-Stokes and temperature convection-diffusion equations are solved together with N additional mass-fractions equations; equations`Navier_Stokes_QC `,  `Convection_Diffusion_Chaleur_QC` and N additional `Convection_Diffusion_Espece_Multi_QC` equations.
	
	Besides, in `Pb_Thermohydraulique_Especes_WC`, the Navier-Stokes and temperature convection-diffusion equations are solved together with N - 1 additional mass-fractions equations; equations`Navier_Stokes_WC `,  `Convection_Diffusion_Chaleur_WC` and N - 1 additional `Convection_Diffusion_Espece_Multi_WC` equations.. The last mass fraction of species N is deduced from the fact of the unity of the sum of the N mass fractions. 
	
# Multi-Phase Problem

The C++ class is `Pb_Multiphase`. This problem allows the resolution of N-phases via a model of 3N equations.

An instance of the medium `Milieu_composite` is required at the begining of the problem's bloc. Inside the medium bloc, you can define any medium you can either define a `Fluide_Incompressible` or any child class of the `Fluide_reel_base` class.

An additional `Correlations` bloc should be **secondly** defined. Al correlations derive from the base class `Corellation_base`. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classCorrelation__base.html)** to see the Doxygen documentation of this class.

Afterwards, the reading of the equations starts. In `Pb_Multiphase`, the equations solved for each phase are the momentum, mass and energy equations; respectively `QDM_Multiphase`, `Masse_Multiphase` and `Energie_Multiphase`.

The coupling between all equations is done in a strong way; ie: a single matrix for all equations is used. This problem is available with all versions of the PolyMAC discretizations and with VDF. Calling **EOS** (private CEA/EDF project) and **[CoolProp](http://www.coolprop.org/)** to compute the Thermo-Physical Properties is possible for this problem via the `TPPI` interface.

# Coupled Problems

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