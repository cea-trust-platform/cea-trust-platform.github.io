Problem definition
==================

Set of equations
----------------

Depending on your choosed problem type, you will have a different set of
equations.

Incompressible problems
~~~~~~~~~~~~~~~~~~~~~~~

**TRUST** solves Navier-Stokes equations with/without the heat equation
for an incompressible fluid:

.. math::

   \left\{
   \begin{array}{c}
   \nabla \cdot \vec u =0 \\
   \displaystyle{\frac{\partial \vec u }{\partial t} + \textcolor{red}{\nabla \cdot (\vec u \otimes \vec u)} = \textcolor{blue}{\nabla \cdot (\nu \nabla \vec u)} - \nabla P^* } \\
   \displaystyle{\frac{\partial T}{\partial t} + \textcolor{red}{\vec u \nabla T} = \textcolor{blue}{\nabla \cdot (\alpha \nabla T)} + \frac{Q}{\rho C_p}}
   \end{array}
   \right.

where: :math:`\displaystyle{P^*=\frac{P}{\rho} + g z}`, :math:`Q` is the
heat source term, and:

-  :math:`\rho`: density,

-  :math:`\mu`: dynamic viscosity,

-  :math:`\displaystyle{\nu=\frac{\mu}{\rho}}`: kinematic viscosity,

-  :math:`\vec g=g z`: gravity vector in cartesian coordinates,

-  :math:`\displaystyle{\alpha=\frac{\lambda}{\rho C_p}}`: thermal
   diffusivity.

-  :math:`C_p`: specific heat capacity at constant pressure,

-  :math:`\lambda`: thermal conductivity,

| **Note** that red terms are convective terms and blue terms are
  diffusive terms.

.. container:: center

| For documentation, see:

+-------------+-------------+-------------+-------------+-------------+
| Thermo      | hydraulique | Co          | Turbulent   | Reference   |
|             |             | ncentration |             | Manual      |
+=============+=============+=============+=============+=============+
|             | **Pb_hy     |             |             | `doc <      |
|             | draulique** |             |             | TRUST_Refer |
|             |             |             |             | ence_Manual |
|             |             |             |             | .pdf#pbhydr |
|             |             |             |             | aulique>`__ |
+-------------+-------------+-------------+-------------+-------------+
|             | **Pb_hy     | **\_Conc    |             | `doc <TR    |
|             | draulique** | entration** |             | UST_Referen |
|             |             |             |             | ce_Manual.p |
|             |             |             |             | df#pbhydrau |
|             |             |             |             | liqueconcen |
|             |             |             |             | tration>`__ |
+-------------+-------------+-------------+-------------+-------------+
|             | **Pb_hy     |             | **\_        | TrioCFD     |
|             | draulique** |             | Turbulent** | Reference   |
|             |             |             |             | Manual      |
+-------------+-------------+-------------+-------------+-------------+
|             | **Pb_hy     | **\_Conc    | **\_        | TrioCFD     |
|             | draulique** | entration** | Turbulent** | Reference   |
|             |             |             |             | Manual      |
+-------------+-------------+-------------+-------------+-------------+
| **P         | **hy        |             |             | `           |
| b\_Thermo** | draulique** |             |             | doc <TRUST_ |
|             |             |             |             | Reference_M |
|             |             |             |             | anual.pdf#p |
|             |             |             |             | bthermohydr |
|             |             |             |             | aulique>`__ |
+-------------+-------------+-------------+-------------+-------------+
| **P         | **hy        | **\_Conc    |             | `do         |
| b\_Thermo** | draulique** | entration** |             | c <TRUST_Re |
|             |             |             |             | ference_Man |
|             |             |             |             | ual.pdf#pbt |
|             |             |             |             | hermohydrau |
|             |             |             |             | liqueconcen |
|             |             |             |             | tration>`__ |
+-------------+-------------+-------------+-------------+-------------+
| **P         | **hy        |             | **\_        | TrioCFD     |
| b\_Thermo** | draulique** |             | Turbulent** | Reference   |
|             |             |             |             | Manual      |
+-------------+-------------+-------------+-------------+-------------+
| **P         | **hy        | **\_Conc    | **\_        | TrioCFD     |
| b\_Thermo** | draulique** | entration** | Turbulent** | Reference   |
|             |             |             |             | Manual      |
+-------------+-------------+-------------+-------------+-------------+

Quasi-compressible problem
~~~~~~~~~~~~~~~~~~~~~~~~~~

**TRUST** solves Navier-Stokes equations with/without heat equation for
quasi-compressible fluid:

.. math::

   \left\{
   \begin{array}{c}
   \displaystyle{\frac{\partial \rho }{\partial t} + \nabla \cdot (\rho \vec u) =0 }\\
   \displaystyle{ \frac{\partial \rho \vec u}{\partial t} + \textcolor{red}{\nabla \cdot (\rho \vec u \vec u)} =  \textcolor{blue}{\nabla \cdot \left(\mu \nabla \vec u \right)} - \nabla P -\rho \vec g }\\
   \displaystyle{ \rho C_p \left( \frac{\partial T}{\partial t} + \textcolor{red}{\vec u \nabla T} \right) = \textcolor{blue}{\nabla \cdot \left(\lambda \nabla T\right)} + \frac{dP_0}{dt} + Q }
   \end{array}
   \right.

where: :math:`P_0=\rho R T`, :math:`Q` is a heat source term, and:

-  :math:`\rho`: density,

-  :math:`\mu`: dynamic viscosity,

-  :math:`\vec g=g z`: gravity vector in cartesian coordinates,

-  :math:`C_p`: specific heat capacity at constant pressure,

-  :math:`\lambda`: thermal conductivity.

| **Note** that red terms are convective terms and blue terms are
  diffusive terms.

.. container:: center

For more information on QC problem, go
`there <TRUST_Reference_Manual.pdf#pbthermohydrauliqueqc>`__ and for
turbulent QC problem see TrioCFD Reference Manual.

Conduction problem
~~~~~~~~~~~~~~~~~~

For this kind of problems, **TRUST** solves the heat equation:

.. math:: \rho C_p \frac{\partial T}{\partial t} = \textcolor{blue}{\nabla \cdot \left(\lambda \nabla T\right)} + Q

where:

-  :math:`\rho`: density,

-  :math:`C_p`: specific heat capacity at constant pressure,

-  :math:`\lambda`: thermal conductivity,

-  :math:`Q` is a heat source term.

| **Note** that term in blue is the diffusive term.
| In your data file, you will have:

.. container:: center

For more information, see the `Project Reference
Manual <TRUST_Reference_Manual.pdf#pbconduction>`__.

Coupled problems
~~~~~~~~~~~~~~~~

| With **TRUST**, we can couple problems. We will explain here the
  method for two problems but you can couple as many problems as you
  want.
| To couple two problems, we define two problems *my_problem_1* and
  *my_problem_2* each one associated to a separate domain *my_domain_1*
  and *my_domain_2*, and to a separate medium *my_medium_1* and
  *my_medium_2* (associated or not to the gravity).

.. container:: center

Then we define a coupled problem associated to a single time scheme like
for example:

.. container:: center

Then we discretize and solve everything:

.. container:: center

You can see the documentation of this kind of problem in the `Project
Reference Manual <TRUST_Reference_Manual.pdf#coupledproblem>`__.

Other problems
~~~~~~~~~~~~~~

**TRUST**\ & TrioCFD can also solve the following types of problems
(Turbulence models in TrioCFD):

-  `Resolution of NAVIER STOKES/energy/multiple constituent
   transportation equations, with the additional passive scalar
   equations <TRUST_Reference_Manual.pdf#pbthermohydrauliqueconcentrationscalairespassifs>`__.

Pressure solvers
----------------

Then you may indicate the choice of pressure solver (cf `Project
Reference Manual <TRUST_Reference_Manual.pdf#solveursysbase>`__) using
the following syntax:

.. container:: center

The *my_solver* may be:

-  `GCP <TRUST_Reference_Manual.pdf#solvgcp>`__,

-  `Petsc Petsc_solver_name <TRUST_Reference_Manual.pdf#petsc>`__,

-  `Cholesky <TRUST_Reference_Manual.pdf#cholesky>`__,

-  `Gmres <TRUST_Reference_Manual.pdf#gmres>`__,

-  `Gen <TRUST_Reference_Manual.pdf#gen>`__,

-  `Optimal <TRUST_Reference_Manual.pdf#optimal>`__.

Reminder: in CFD, a separate solver is used to solve the pressure. For
more details, you can have a look at the section "Time and space
schemes" of the **TRUST**\ & **TrioCFD** user slides.

Convection
----------

There is no default convective scheme so you must choose one `convection
scheme <TRUST_Reference_Manual.pdf#blocconvection>`__:

.. container:: center

You can use the following convective scheme, following the
recommendations of the user training session (cf section "Time and space
schemes" of the **TRUST**\ & **TrioCFD** user slides and the section
"Recommendations for schemes") following your discretization type:

-  `Amont <TRUST_Reference_Manual.pdf#convectionamont>`__

-  `Muscl <TRUST_Reference_Manual.pdf#convectionmuscl>`__

-  `EF_stab <TRUST_Reference_Manual.pdf#convectionefstab>`__

-  for more, see the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#blocconvection>`__.

**Note** that there is no default convective scheme and if you don’t
want convection in your problem, you may use:

.. container:: center

Diffusion
---------

For the diffusive scheme, it is the same syntax:

.. container:: center

| You can choose your scheme with the help of the `Project Reference
  Manual <TRUST_Reference_Manual.pdf#blocdiffusion>`__.
| **Note** that if you don’t specify any diffusive scheme, the code
  automatically uses the standard diffusive scheme of order 2. If you
  don’t want diffusion in your problem, you may use:

.. container:: center

Initial conditions
------------------

For each equation, you **must** set initial conditions:

.. container:: center

To see the syntax of each available initial condition: cf `Project
Reference Manual <TRUST_Reference_Manual.pdf#condinits>`__. Here are the
most used initial conditions:

-  **Vitesse** field_type *bloc_lecture_champ*

-  **Temperature** field_type *bloc_lecture_champ*

-  **K_eps** field_type *bloc_lecture_champ*

We list here some "field_type":

-  `Uniform_Field <TRUST_Reference_Manual.pdf#uniformfield>`__: for a
   uniform field,

-  `Champ_Fonc_Med <TRUST_Reference_Manual.pdf#champfoncmed>`__: to read
   a data field in a MED-format file .med at a specified time,

-  `Champ_Fonc_txyz <TRUST_Reference_Manual.pdf#fieldfunctxyz>`__: for a
   field which depends on time and space,

-  `Champ_Fonc_Fonction_txyz <TRUST_Reference_Manual.pdf#champfoncfonctiontxyz>`__:
   for a field which is a function of another field and time and/or
   space coordinates,

-  `Champ_Fonc_Reprise <TRUST_Reference_Manual.pdf#champfoncreprise>`__:
   to read a data field in a saved file (.xyz or .sauv) at a specified
   time.

-  refer to the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#fieldbase>`__.

Boundary conditions
-------------------

Then you may specify your boundary conditions like:

.. container:: center

| It is important to specify here that **TRUST will not accept any
  boundary conditions by default.**
| You can find help for boundary conditions in the `Project Reference
  Manual <TRUST_Reference_Manual.pdf#condlimbase>`__. Here is a list of
  the most used boundary conditions:

-  `Bord
   Frontiere_ouverte_vitesse_imposee <TRUST_Reference_Manual.pdf#frontiereouvertevitesseimposee>`__
   boundary_field_type *bloc_lecture_champ*

-  `Bord
   Frontiere_ouverte_pression_imposee <TRUST_Reference_Manual.pdf#frontiereouvertepressionimposee>`__
   boundary_field_type *bloc_lecture_champ*

-  `Bord Paroi_fixe <TRUST_Reference_Manual.pdf#paroifixe>`__

-  `Bord Symetrie <TRUST_Reference_Manual.pdf#symetrie>`__

-  `Bord Periodique <TRUST_Reference_Manual.pdf#periodic>`__

-  `Bord
   Frontiere_ouverte_temperature_imposee <TRUST_Reference_Manual.pdf#frontiereouvertetemperatureimposee>`__
   boundary_field_type *bloc_lecture_champ*

-  `Bord Frontiere_ouverte
   T_ext <TRUST_Reference_Manual.pdf#frontiereouverte>`__
   boundary_field_type *bloc_lecture_champ*

-  `Bord
   Paroi_adiabatique <TRUST_Reference_Manual.pdf#paroiadiabatique>`__

-  `Bord
   Paroi_flux_impose <TRUST_Reference_Manual.pdf#paroifluximpose>`__
   boundary_field_type *bloc_lecture_champ*

-  for more, see the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#condlimbase>`__.

To choose your "boundary_field_type" parameters, refer to the `Project
Reference Manual <TRUST_Reference_Manual.pdf#frontfieldbase>`__.

Turbulence models (in TrioCFD)
------------------------------

User can add a turbulence model to his simulation using the keyword:

.. container:: center

where *my_model* can be:

-  `Longueur_Melange <TRUST_Reference_Manual.pdf#longueurmelange>`__:
   RANS model based on mixing length modelling,

-  `Sous_maille <TRUST_Reference_Manual.pdf#sousmaille>`__: LES model
   which uses a structure sub-grid function model,

-  `K_epsilon <TRUST_Reference_Manual.pdf#kepsilon>`__: for RANS
   turbulence model (k-:math:`\varepsilon`),

-  for more, see the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#modeleturbulencehydderiv>`__.

Source terms
------------

To introduce a source term into an equation, add the following line into
the block defining the equation. The list of source keyword is described
below.

.. container:: center

To introduce several source terms into the same equation, the blocks
corresponding to the various terms need to be separated by a comma:

.. container:: center

-  `Perte_Charge_Reguliere <TRUST_Reference_Manual.pdf#pertechargereguliere>`__
   type_perte_charge bloc_definition_pertes_charges

-  `Perte_Charge_Singuliere <TRUST_Reference_Manual.pdf#pertechargesinguliere>`__
   **KX \| KY \| KZ** coefficient_value { ... }

-  `Canal_perio <TRUST_Reference_Manual.pdf#canalperio>`__ { ... }

-  `Boussinesq_temperature <TRUST_Reference_Manual.pdf#boussinesqtemperature>`__
   { ... }: :math:`\rho(T)=\rho(T_0)(1-\beta_{th}(T-T_0))`

-  `Boussinesq_concentration <TRUST_Reference_Manual.pdf#boussinesqconcentration>`__
   { ... }

-  `Puissance_thermique <TRUST_Reference_Manual.pdf#puissancethermique>`__
   field_type bloc_lecture_champ

-  `documentation for hydraulic source terms and for scalar source
   terms <TRUST_Reference_Manual.pdf#sourcebase>`__.

Post-processing
---------------

| Before post-processing fields, during a run, **TRUST** creates several
  files which contain information about the calculation, the
  convergence, fluxes, balances... (see part `6.1 <#post>`__ for more
  information).
| Several keywords can be used to create a post-processing block, into a
  problem. First, you can create a single post-processing task
  (`Post_processing <TRUST_Reference_Manual.pdf#postraitement>`__
  keyword). Generally, in this block, results will be printed with a
  specified format at a specified time period.

.. container:: center

But you can also create a list of post-processings with
`Post_processings <TRUST_Reference_Manual.pdf#postraitements>`__ keyword
(named with Post_name1, Post_name2, etc...), in order to print results
into several formats or with different time periods, or into different
results files:

.. container:: center

Field names
~~~~~~~~~~~

-  Existing & predefined fields

   You can post-process predefined fields and already existing fields.
   Here is a list of post-processable fields, but it is not the only
   ones.

   +----------------------+----------------------+----------------------+
   | **Physical values**  | **Keyword for        | **Unit**             |
   |                      | field_name**         |                      |
   +======================+======================+======================+
   | ... continued on     |                      |                      |
   | next page ...        |                      |                      |
   +----------------------+----------------------+----------------------+
   | Velocity             | **Vitesse** or       | :math:`m.s^{-1}`     |
   |                      | **Velocity**         |                      |
   +----------------------+----------------------+----------------------+
   | Velocity residual    | **Vitesse_residu**   | :math:`m.s^{-2}`     |
   +----------------------+----------------------+----------------------+
   | Kinetic energy per   |                      |                      |
   | elements             |                      |                      |
   +----------------------+----------------------+----------------------+
   | (:math:`             | **Ener               | :mat                 |
   | 0.5 \rho ||u_i||^2`) | gie_cinetique_elem** | h:`kg.m^{-1}.s^{-2}` |
   +----------------------+----------------------+----------------------+
   | Total kinetic energy |                      |                      |
   +----------------------+----------------------+----------------------+
   | :math:`\displaystyl  | **Energi             | :mat                 |
   | e \left( \frac{\sum_ | e_cinetique_totale** | h:`kg.m^{-1}.s^{-2}` |
   | {i=1}^{nb\_elem} 0.5 |                      |                      |
   |  \rho ||u_i||^2 vol_ |                      |                      |
   | i}{\sum_{i=1}^{nb\_e |                      |                      |
   | lem} vol_i} \right)` |                      |                      |
   +----------------------+----------------------+----------------------+
   | Vorticity            | **Vorticite**        | :math:`s^{-1}`       |
   +----------------------+----------------------+----------------------+
   | Pressure in          |                      |                      |
   | incompressible flow  |                      |                      |
   +----------------------+----------------------+----------------------+
   | (:math:`P/\rho+gz`)  | **Pression**  [7]_   | :m                   |
   |                      |                      | ath:`Pa.m^3.kg^{-1}` |
   +----------------------+----------------------+----------------------+
   | For Front Tracking   |                      | or                   |
   | probleme             |                      |                      |
   +----------------------+----------------------+----------------------+
   | (:math:`P+\rho gz`)  |                      | :math:`Pa`           |
   +----------------------+----------------------+----------------------+
   | Pressure in          |                      |                      |
   | incompressible flow  |                      |                      |
   +----------------------+----------------------+----------------------+
   | (                    | **Pression_pa** or   | :math:`Pa`           |
   | P+\ :math:`\rho gz`) | **Pressure**         |                      |
   +----------------------+----------------------+----------------------+
   | Pressure in          | **Pression**         | :math:`Pa`           |
   | compressible flow    |                      |                      |
   +----------------------+----------------------+----------------------+
   | Hydrostatic pressure | **Pres               | :math:`Pa`           |
   | :math:`(\rho g z)`   | sion_hydrostatique** |                      |
   +----------------------+----------------------+----------------------+
   | Totale pressure      |                      |                      |
   | (when                |                      |                      |
   +----------------------+----------------------+----------------------+
   | quasi compressible   |                      |                      |
   | model                |                      |                      |
   +----------------------+----------------------+----------------------+
   | is used)=Pth+P       | **Pression_tot**     | :math:`Pa`           |
   +----------------------+----------------------+----------------------+
   | Pressure gradient    |                      |                      |
   +----------------------+----------------------+----------------------+
   | (:math:              | *                    | :math:`m.s^{-2}`     |
   | `\nabla(P/\rho+gz)`) | *Gradient_pression** |                      |
   +----------------------+----------------------+----------------------+
   | Velocity gradient    | **gradient_vitesse** | :math:`s^{-1}`       |
   +----------------------+----------------------+----------------------+
   | Temperature          | **Temperature**      | :math:`^o`\ C or K   |
   +----------------------+----------------------+----------------------+
   | Temperature residual | **                   | :math:`^o`           |
   |                      | Temperature_residu** | \ C.\ :math:`s^{-1}` |
   |                      |                      | or                   |
   |                      |                      | K.\ :math:`s^{-1}`   |
   +----------------------+----------------------+----------------------+
   | Phase temperature of |                      |                      |
   +----------------------+----------------------+----------------------+
   | a two phases flow    | **Temper             | :math:`^o`\ C or K   |
   |                      | ature_EquationName** |                      |
   +----------------------+----------------------+----------------------+
   | Mass transfer rate   |                      |                      |
   +----------------------+----------------------+----------------------+
   | between two phases   | **                   | :mat                 |
   |                      | Temperature_mpoint** | h:`kg.m^{-2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Temperature variance | **Va                 | :math:`K^2`          |
   |                      | riance_Temperature** |                      |
   +----------------------+----------------------+----------------------+
   | Temperature          | **Taux_Dissi         | :math:`K^2.s^{-1}`   |
   | dissipation rate     | pation_Temperature** |                      |
   +----------------------+----------------------+----------------------+
   | Temperature gradient | **Gr                 | :math:`K.m^{-1}`     |
   |                      | adient_temperature** |                      |
   +----------------------+----------------------+----------------------+
   | Heat exchange        | **H_echange_Tref**   | :ma                  |
   | coefficient          |  [8]_                | th:`W.m^{-2}.K^{-1}` |
   +----------------------+----------------------+----------------------+
   | Turbulent heat flux  | **Flux_              | :math:`m.K.s^{-1}`   |
   |                      | Chaleur_Turbulente** |                      |
   +----------------------+----------------------+----------------------+
   | Turbulent viscosity  | **Vi                 | :math:`m^2.s^{-1}`   |
   |                      | scosite_turbulente** |                      |
   +----------------------+----------------------+----------------------+
   | Turbulent dynamic    |                      |                      |
   | viscosity            |                      |                      |
   +----------------------+----------------------+----------------------+
   | (when quasi          | **Viscosite_dy       | :math:`kg.m.s^{-1}`  |
   | compressible         | namique_turbulente** |                      |
   +----------------------+----------------------+----------------------+
   | model is used)       |                      |                      |
   +----------------------+----------------------+----------------------+
   | Turbulent kinetic    | **K**                | :math:`m^2.s^{-2}`   |
   | energy               |                      |                      |
   +----------------------+----------------------+----------------------+
   | Turbulent            | **Eps**              | :math:`m^3.s^{-1}`   |
   | dissipation rate     |                      |                      |
   +----------------------+----------------------+----------------------+
   | Turbulent quantities |                      |                      |
   +----------------------+----------------------+----------------------+
   | K and Epsilon        | **K_Eps**            | (:math:`m^2.s^{-2}`  |
   |                      |                      | ,                    |
   |                      |                      | \ :math:`m^3.s^{-1}` |
   |                      |                      | )                    |
   +----------------------+----------------------+----------------------+
   | Residuals of         |                      |                      |
   | turbulent quantities |                      |                      |
   +----------------------+----------------------+----------------------+
   | K and Epsilon        | **K_Eps_residu**     | (:math:`m^2.s^{-3}`  |
   | residuals            |                      | ,                    |
   |                      |                      | \ :math:`m^3.s^{-2}` |
   |                      |                      | )                    |
   +----------------------+----------------------+----------------------+
   | Constituent          | **Concentration**    |                      |
   | concentration        |                      |                      |
   +----------------------+----------------------+----------------------+
   | Constituent          | **Co                 |                      |
   | concentration        | ncentration_residu** |                      |
   | residual             |                      |                      |
   +----------------------+----------------------+----------------------+
   | Component velocity   | **VitesseX**         | :math:`m.s^{-1}`     |
   | along X              |                      |                      |
   +----------------------+----------------------+----------------------+
   | Component velocity   | **VitesseY**         | :math:`m.s^{-1}`     |
   | along Y              |                      |                      |
   +----------------------+----------------------+----------------------+
   | Component velocity   | **VitesseZ**         | :math:`m.s^{-1}`     |
   | along Z              |                      |                      |
   +----------------------+----------------------+----------------------+
   | Mass balance on each | **Divergence_U**     | :math:`m^3.s^{-1}`   |
   | cell                 |                      |                      |
   +----------------------+----------------------+----------------------+
   | Irradiancy           | **Irradiance**       | :math:`W.m^{-2}`     |
   +----------------------+----------------------+----------------------+
   | Q-criteria           | **Critere_Q**        | :math:`s^{-1}`       |
   +----------------------+----------------------+----------------------+
   | Distance to the wall |                      |                      |
   | :math:`Y^+=yU/\nu`   |                      |                      |
   +----------------------+----------------------+----------------------+
   | (only computed on    | **Y_plus**           | dimensionless        |
   +----------------------+----------------------+----------------------+
   | boundaries of wall   |                      |                      |
   | type)                |                      |                      |
   +----------------------+----------------------+----------------------+
   | Friction velocity    | **U_star**           | :math:`m.s^{-1}`     |
   +----------------------+----------------------+----------------------+
   | Void fraction        | **alpha**            | dimensionless        |
   +----------------------+----------------------+----------------------+
   | Cell volumes         | **Volume_maille**    | :math:`m^3`          |
   +----------------------+----------------------+----------------------+
   | Chemical potential   | **Potentiel_C        |                      |
   |                      | himique_Generalise** |                      |
   +----------------------+----------------------+----------------------+
   | Source term in non   |                      |                      |
   +----------------------+----------------------+----------------------+
   | Galinean referential | **Acceler            | :math:`m.s^{-2}`     |
   |                      | ation_terme_source** |                      |
   +----------------------+----------------------+----------------------+
   | Stability time steps | **Pas_de_temps**     | S                    |
   +----------------------+----------------------+----------------------+
   | Listing of boundary  | **Flux_bords**       | cf each \*.out file  |
   | fluxes               |                      |                      |
   +----------------------+----------------------+----------------------+
   | Volumetric porosity  | **                   | dimensionless        |
   |                      | Porosite_volumique** |                      |
   +----------------------+----------------------+----------------------+
   | Distance to the wall | **Distance_Paroi**   | :math:`m`            |
   |                      |  [9]_                |                      |
   +----------------------+----------------------+----------------------+
   | Volumic thermal      | **P                  | :math:`W.m^{-3}`     |
   | power                | uissance_volumique** |                      |
   +----------------------+----------------------+----------------------+
   | Local shear strain   |                      |                      |
   | rate defined as      |                      |                      |
   +----------------------+----------------------+----------------------+
   | :mat                 | *                    | :math:`s^{-1}`       |
   | h:`\sqrt{(2SijSij)}` | *Taux_cisaillement** |                      |
   +----------------------+----------------------+----------------------+
   | Cell Courant number  | **Courant_maille**   | dimensionless        |
   | (VDF only)           |                      |                      |
   +----------------------+----------------------+----------------------+
   | Cell Reynolds number | **Reynolds_maille**  | dimensionless        |
   | (VDF only)           |                      |                      |
   +----------------------+----------------------+----------------------+
   | Viscous force        | **viscous_force**    | :ma                  |
   |                      |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Pressure force       | **pressure_force**   | :ma                  |
   |                      |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Total force          | **total_force**      | :ma                  |
   |                      |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Viscous force along  | **viscous_force_x**  | :ma                  |
   | X                    |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Viscous force along  | **viscous_force_y**  | :ma                  |
   | Y                    |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Viscous force along  | **viscous_force_z**  | :ma                  |
   | Z                    |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Pressure force along | **pressure_force_x** | :ma                  |
   | X                    |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Pressure force along | **pressure_force_y** | :ma                  |
   | Y                    |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Pressure force along | **pressure_force_z** | :ma                  |
   | Z                    |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Total force along X  | **total_force_x**    | :ma                  |
   |                      |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Total force along Y  | **total_force_y**    | :ma                  |
   |                      |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+
   | Total force along Z  | **total_force_z**    | :ma                  |
   |                      |                      | th:`kg.m^{2}.s^{-1}` |
   +----------------------+----------------------+----------------------+

   *Remark:* Physical properties (conductivity, diffusivity,...) can
   also be post-processed.

   **The name of the fields and components available for post-processing
   is displayed in the error file after the following message: "Reading
   of fields to be postprocessed". Of course, this list depends of the
   problem being solved.**

   For more information, you can see the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#champsapost>`__.

-  Creating new fields

   The
   `Definition_champs <TRUST_Reference_Manual.pdf#definitionchamps>`__
   keyword is used to create new or more complex fields for advanced
   post-processing.

   .. container:: center

   *field_name_post* is the name of the new created field and
   **field_type** is one of the following possible type:

   -  `refChamp <TRUST_Reference_Manual.pdf#refchamp>`__

   -  `Reduction_0D <TRUST_Reference_Manual.pdf#reduction0d>`__ using
      for example the **min**, **max** or **somme** methods.

   -  `Transformation <TRUST_Reference_Manual.pdf#transformation>`__

   -  for details and other keywords, see the `Project Reference
      Manual <TRUST_Reference_Manual.pdf#definitionchamps>`__.

   | **Note** that you can combine several **field_type** keywords to
     create your field and then use your new fields to create other
     ones.
   | Here is an example of new field named *max_temperature*:

   .. container:: center

   You can find other examples in the **TRUST**\ & **TrioCFD** user
   slides in the section "Post processing description".

Post-processing blocks
~~~~~~~~~~~~~~~~~~~~~~

There are three methods to post-process in **TRUST**: using probes,
fields or making statistics.

-  | Probes
   | Probes refer to sensors that allow a value or several points of the
     domain to be monitored over time. The probes are a set of points
     defined:

   -  one by one: `Points <TRUST_Reference_Manual.pdf#points>`__ keyword
      or

   -  by a set of points evenly distributed over a straight segment:
      `Segment <TRUST_Reference_Manual.pdf#segment>`__ keyword or

   -  arranged according to a layout:
      `Plan <TRUST_Reference_Manual.pdf#plan>`__ keyword or

   -  arranged according to a parallelepiped:
      `Volume <TRUST_Reference_Manual.pdf#volume>`__ keyword.

   Here is an example of 2D **Probes** block:

   .. container:: center

   where the use of *"loc"* option allow to specify the wanted location
   of the probes. The available values are **"grav"** for gravity center
   of the element, **"nodes"** for faces and **"som"** for vertices.
   There is not default location. If the point does not coincide with a
   calculation node, the value is extrapolated linearly according to
   neighbouring node values.

   For complete syntax, see the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#corpspostraitement>`__, also for
   `all options <TRUST_Reference_Manual.pdf#sondes>`__.

-  | Fields
   | This keyword allows to post-process fields on the whole domain,
     specifying the name of the backup file, its format, the
     post-processing time step and the name (and location) of the
     post-processed fields.

   Here is an example of
   `Fields <TRUST_Reference_Manual.pdf#champsposts>`__ block:

   .. container:: center

   | where **"faces"** , **"elem"** and **"som"** are keywords allowed
     to specify the location of the field.
   | **Note** that *when you don’t specify the location of the field*,
     the default value is **"som"** for values at the vertices. So
     fields are post-processed at the vertices of the mesh.
   | To visualize your post-processed fields, you can use open source
     softwares like:
     `VisIt <https://wci.llnl.gov/simulation/computer-codes/visit>`__
     (included in **TRUST** package) for lata and med files, or for med
     files: `Salomé <http://www.salome-platform.org>`__ or
     `Paraview <http://www.paraview.org>`__.
   | You can see the `complete
     syntax <TRUST_Reference_Manual.pdf#corpspostraitement>`__ and `all
     options <TRUST_Reference_Manual.pdf#champsposts>`__ in the `Project
     Reference Manual <TRUST_Reference_Manual.pdf>`__.

-  | Statistics
   | Using this keyword, you will compute statistics on your unknows.
     You must specify the begining and ending time for the statistics,
     the post-processing time step, the statistic method, the name (and
     location) of your post-processed field.

   Here is an example of
   `Statistiques <TRUST_Reference_Manual.pdf#statsposts>`__ block:

   .. container:: center

   This block will write at every **dt_post** the average of the
   velocity :math:`\overline{V(t)}`:

   .. math::

      \overline{V(t)}=\left\{ \begin{array}{ll}
      0 & ,\mbox{ for }t\leq t\mbox{\_}deb\\
      \frac{1}{t-t\mbox{\_}deb}{\displaystyle \int_{t\mbox{\_}deb}^{t}V(t)dt} & ,\mbox{ for }t\mbox{\_}deb<t\leq t\mbox{\_}fin\\
      \frac{1}{t\mbox{\_}fin-t\mbox{\_}deb}{\displaystyle \int_{t\mbox{\_}deb}^{t\mbox{\_}fin}V(t)dt} & ,\mbox{ for }t>t\mbox{\_}fin
      \end{array}\right.

   the standard deviation of the pressure
   :math:`\left\langle P(t)\right\rangle`:

   .. math::

      \left\langle P(t)\right\rangle=\left\{ \begin{array}{ll}
      0 & ,\mbox{ for }t\leq t\mbox{\_}deb\\
      \frac{1}{t-t\mbox{\_}deb}{\displaystyle \sqrt{\int_{t\mbox{\_}deb}^{t}\left[P(t)-\overline{P(t)}\right]^{2}dt}} & ,\mbox{ for }t\mbox{\_}deb<t\leq t\mbox{\_}fin\\
      \frac{1}{t\mbox{\_}fin-t\mbox{\_}deb}{\displaystyle \sqrt{\int_{t\mbox{\_}deb}^{t\mbox{\_}fin}\left[P(t)-\overline{P(t)}\right]^{2}dt}} & ,\mbox{ for }t>t\mbox{\_}fin
      \end{array}\right.

   and correlation between the pressure and the velocity
   :math:`\left\langle P(t).V(t)\right\rangle` like:

   .. math::

      \left\langle P(t).V(t)\right\rangle=\left\{ \begin{array}{ll}
      0 & ,\mbox{ for }t\leq t\mbox{\_}deb\\
      \frac{1}{t-t\mbox{\_}deb}{\displaystyle \int_{t\mbox{\_}deb}^{t}\left[P(t)-\overline{P(t)}\right]\cdot\left[V(t)-\overline{V(t)}\right]dt} & ,\mbox{ for }t\mbox{\_}deb<t\leq t\mbox{\_}fin\\
      \frac{1}{t\mbox{\_}fin-t\mbox{\_}deb}{\displaystyle \int_{t\mbox{\_}deb}^{t\mbox{\_}fin}\left[P(t)-\overline{P(t)}\right]\cdot\left[V(t)-\overline{V(t)}\right]dt} & ,\mbox{ for }t>t\mbox{\_}fin
      \end{array}\right.

   | *Remark:* Statistical fields can be plotted with probes with the
     keyword "operator_field_name" like for example: Moyenne_Vitesse or
     Ecart_Type_Pression or Correlation_Vitesse_Vitesse. For that, it is
     mandatory to have the statistical calculation of this fields
     defined with the keyword **Statistiques**.
   | For the complete syntax, see the Project Reference Manual
     `here <TRUST_Reference_Manual.pdf#corpspostraitement>`__, and for
     all options see the `Project Reference
     Manual <TRUST_Reference_Manual.pdf#statsposts>`__.

Post-process location
~~~~~~~~~~~~~~~~~~~~~

| You can use location keywords to specify where you want to
  post-process your fields in order to avoid interpolations on your
  post-processed fields.
| For the **VDF** discretization, you can see the Figure
  `[fig_VDF] <#fig_VDF>`__ and here is a table with a reminder of the
  computation location of the fields in the **VDF** discretization:

+-------------------+-------------------+-------------------+---+---+
| **Names**         | **Keyword**       | **Location**      |   |   |
+===================+===================+===================+===+===+
| Pressure          | **pressure**      | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| Velocity          | **velocity**      | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| Temperature       | **temperature**   | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| Density           | **                | element gravity   |   |   |
| :math:`\rho`      | masse_volumique** | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| Kinematic         | **viscos          | element gravity   |   |   |
| viscosity         | ite_cinematique** | center            |   |   |
| :math:`\nu`       |                   |                   |   |   |
+-------------------+-------------------+-------------------+---+---+
| Dynamic viscosity | **visc            | element gravity   |   |   |
| :math:`\mu`       | osite_dynamique** | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| K                 | **k**             | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| eps               | **eps**           | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| :math:`y^+`       | **y_plus**        | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| :math:`u^*`       | **u_star**        | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| Turbulent         | **visco           | element gravity   |   |   |
| viscosity         | site_turbulente** | center            |   |   |
+-------------------+-------------------+-------------------+---+---+

For the **VEFPreP1B** discretization, you can see the Figure
`[fig_VEF] <#fig_VEF>`__ and
`[fig_VEF_pressure_loc] <#fig_VEF_pressure_loc>`__. Here is a table with
a reminder of the computation location of the fields in **VEFPreP1B**
discretization:

+-------------------+-------------------+-------------------+---+---+
| **Names**         | **Keyword**       | **Location**      |   |   |
+===================+===================+===================+===+===+
|                   |                   | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| Pressure          | **pressure**      | vertices          |   |   |
+-------------------+-------------------+-------------------+---+---+
| Velocity          | **velocity**      | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| Temperature       | **temperature**   | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| Density           | **                | element gravity   |   |   |
| :math:`\rho`      | masse_volumique** | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| Kinematic         | **viscos          | element gravity   |   |   |
| viscosity         | ite_cinematique** | center            |   |   |
| :math:`\nu`       |                   |                   |   |   |
+-------------------+-------------------+-------------------+---+---+
| Dynamic viscosity | **visc            | element gravity   |   |   |
| :math:`\mu`       | osite_dynamique** | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| K                 | **k**             | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| eps               | **eps**           | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| :math:`y^+`       | **y_plus**        | element gravity   |   |   |
|                   |                   | center            |   |   |
+-------------------+-------------------+-------------------+---+---+
| :math:`u^*`       | **u_star**        | faces center      |   |   |
+-------------------+-------------------+-------------------+---+---+
| Turbulent         | **visco           | element gravity   |   |   |
| viscosity         | site_turbulente** | center            |   |   |
+-------------------+-------------------+-------------------+---+---+

| **Be careful**, if you are in P0+P1 discretization (default option)
  and you post-process the pressure field at the element (or at the
  vertices), you will have an **interpolation** because the field is
  computed at the element **and** at the vertices.