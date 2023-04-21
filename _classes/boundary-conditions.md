---
title: "Boundary Conditions"
weight: 6
---

There is no sense to talk about numerical simulations without talking about the boundary conditions ! In order to close the discretized the system, consistent boundary conditions must be defined.

The platform TRUST implements a huge number of boundary conditions (BC) that all inherit from the C++ class `Cond_lim_base`. Here are the available classes that can be used to define your BC's in a TRUST simulation.

# Dirichlet-type Boundary Conditions

- Fluid Inlet BCs

	- `Frontiere_ouverte_alpha_impose`: Imposed void fraction condition at an open boundary called bord (edge) (situation corresponding to a fluid inlet). This condition must be associated with an imposed inlet velocity condition.

	- `Frontiere_ouverte_concentration_imposee`: Imposed concentration condition at an open boundary called bord (edge) (situation corresponding to a fluid inlet). This condition must be associated with an imposed inlet velocity condition.

	- `Frontiere_ouverte_fraction_massique_imposee`: Imposed mass fraction condition at an open boundary called bord (edge) (situation corresponding to a fluid inlet). This condition must be associated with an imposed inlet velocity condition.

	- `Entree_temperature_imposee_H`: Particular case of class frontiere_ouverte_temperature_imposee for enthalpy equation.

	- `Frontiere_ouverte_temperature_imposee`: Imposed temperature condition at the open boundary called bord (edge) (in the case of fluid inlet). This condition must be associated with an imposed inlet velocity condition. The imposed temperature value is expressed in C or K.

	- `Frontiere_ouverte_vitesse_imposee`: Class for velocity-inlet boundary condition. The imposed velocity field at the inlet is vectorial and the imposed velocity values are expressed in m.s-1.

	- `Frontiere_ouverte_rho_u_impose`: This keyword is used to designate a condition of imposed mass rate at an open boundary called bord (edge). The imposed mass rate field at the inlet is vectorial and the imposed velocity values are
expressed in kg.s-1. This boundary condition can be used only with the dilatable model (Low Mach Number).

- Dirichlet BCs

	- `Dirichlet_loi_paroi`: Used for Multiphase Problem with wall laws.

	- `Paroi_defilante`: Keyword to designate a condition where tangential velocity is imposed on the wall called bord (edge). If the velocity components set by the user is not tangential, projection is used.

	- `Paroi_Knudsen_non_negligeable`: Boundary condition for number of Knudsen (Kn) above 0.001 where slip-flow condition appears: the velocity near the wall depends on the shear stress : Kn = l/L with l is the mean-free-path of the
molecules and L a characteristic length scale.

	- `Scalaire_impose_paroi`: Imposed temperature condition at the wall called bord (edge).

	- `Temperature_imposee_paroi`: Imposed temperature condition at the wall called bord (edge).

	- `Paroi_fixe`: Keyword to designate a situation of adherence to the wall called bord (edge) (normal and tangential velocity at the edge is zero).

	- `Paroi`: Impermeability condition at a wall called bord (edge) (standard flux zero). This condition must be associated with a wall type hydraulic condition (`Dirichlet_paroi_fixe`).

	- `Paroi_Temperature_imposee`: Imposed temperature condition at the wall called bord (edge).
	
- Navier BCs

	- `Frottement_externe_impose`: External friction BC used in Multi-Phase problems.

	- `Frottement_global_impose`: Global friction BC used in Multi-Phase problems.

	- `Symetrie`: For Navier-Stokes equations, this keyword is used to designate a symmetry condition concerning the velocity at the boundary called bord (edge) (normal velocity at the edge equal to zero and tangential velocity gradient at the edge equal to zero). For scalar transport equation, this keyword is
used to set a symmetry condition on scalar on the boundary named bord (edge).

- Periodic BCs

	- `Periodique`: For Navier-Stokes equations, this keyword is used to indicate that the horizontal inlet velocity values are the same as the outlet velocity values, at every moment. As regards meshing, the inlet and outlet edges bear the same name. For scalar transport equation, this keyword is used to set a periodic
condition on scalar. The two edges dealing with this periodic condition bear the same name.

# Neumann-type Boundary Conditions

- Wall BCs
	
	- `Neumann_paroi_adiabatique`: Adiabatic wall neumann boundary condition.
		
	- `Paroi_adiabatique`: Normal zero flux condition at the wall called bord (edge).
	
	- `Paroi_flux_impose`: Normal flux condition at the wall called bord (edge). The surface area of the flux (W.m-1 in 2D or W.m-2 in 3D) is imposed at the boundary according to the following convention: a positive flux is a flux that enters into the domain according to convention.

- Outlet BCs

	- `Neumann_sortie_libre_Temp_H`: Open boundary for heat equation with enthalpy as unknown.

	- `Frontiere_ouverte_Gradient_Pression_impose`: Normal imposed pressure gradient condition on the open boundary called bord (edge). This boundary condition may be only used in VDF discretization. The imposed pressure gradient value is expressed in Pa.m-1.

	- `Frontiere_ouverte_pression_imposee`: Imposed pressure condition at the open boundary called bord (edge). The imposed pressure field is expressed in Pa.

	- `Frontiere_ouverte_Pression_imposee_Orlansky`: This boundary condition may only be used with VDF discretization. There is no reference for pressure for this boundary condition so it is better to add pressure condition (with `Frontiere_ouverte_pression_imposee`) on one or two cells (for symetry in a channel) of the boundary where Orlansky conditions are imposed.

	- `Frontiere_ouverte_Gradient_Pression_libre`: Class for outlet boundary condition in VEF like Orlansky. There is no reference for pressure for theses boundary conditions so it is better to add pressure condition (with `Frontiere_ouverte_pression_imposee`) on one or two cells (for symmetry in a channel) of the boundary where Orlansky conditions are imposed.

	- `Frontiere_ouverte_pression_moyenne_imposee`: Class for open boundary with pressure mean level imposed.

	- `Frontiere_ouverte_vitesse_imposee_sortie`: Sub-class for velocity boundary condition. The imposed velocity field at the open boundary is vectorial and the imposed velocity values are expressed in m.s-1.

# Coupling-type Boundary Conditions

- `Echange_couplage_thermique`: Thermal coupling boundary condition.

- `Paroi_contact`: Thermal condition between two domains. Important: the name of the boundaries in the two domains should be the same. (Warning: there is also an old limitation not yet fixed on the sequential algorithm in VDF to detect the matching faces on the two boundaries: faces should be ordered in the same
way). The kind of condition depends on the discretization. In VDF, it is a heat exchange condition, and in VEF, a temperature condition.
Such a coupling requires coincident meshes for the moment. In case of non-coincident meshes, run is stopped and two external files are automatically generated in VEF (`connectivity_failed_boundary_name` and `connectivity_failed_pb_name.med`). In 2D, the keyword `Decouper_bord_coincident` associated to the `connectivity_failed_boundary_name` file allows to generate a new coincident mesh. In 3D, for a first preliminary cut domain with HOMARD (fluid for instance), the second problem associated to pb_name (solide in a fluid/solid coupling problem) has to be submitted to HOMARD cutting procedure with `connectivity_failed_pb_name.med`.

- `Paroi_contact_fictif`: This keyword is derivated from paroi_contact and is especially dedicated to compute coupled fluid/solid/fluid problem in case of thin material. Thanks to this option, solid is considered as a fictitious
media (no mesh, no domain associated), and coupling is performed by considering instantaneous thermal equilibrium in it (for the moment).

- `Echange_contact_VDF`: Boundary condition type to model the heat flux between two problems. Important: the name of the boundaries in the two problems should be the same.

# Robin-type Boundary Conditions

- `Echange_externe_impose`: External type exchange condition with a heat exchange coefficient and an imposed external temperature.

- `Echange_externe_impose_H`: Particular case of class `paroi_echange_externe_impose` for enthalpy equation.

- `Echange_interne_impose`: Internal heat exchange boundary condition with exchange coefficient.

- `Echange_interne_parfait`: Internal heat exchange boundary condition with perfect (infinite) exchange coefficient.

- `Echange_global_impose`: Global type exchange condition (internal) that is to say that diffusion on the first fluid mesh is not taken into consideration.

- `Echange_contact_Correlation`: Class to define a thermohydraulic 1D model which will apply to a boundary of 2D or 3D domain.

- `Echange_interne_global_impose`: Internal heat exchange boundary condition with global exchange coefficient.

- `Echange_interne_global_parfait`: Internal heat exchange boundary condition with perfect (infinite) exchange coefficient.
