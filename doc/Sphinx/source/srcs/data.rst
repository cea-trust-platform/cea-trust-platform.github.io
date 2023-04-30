Data setting
============

We will now explain how to fill a data file. First you must specify some
basic information like the dimension of your domain, its name, the
problem type... To define the problem dimension, we use the following
keyword:

.. container:: center

or

.. container:: center

.. _pbs:

Problems
--------

You have to define the problem type that you wish to solve.

.. container:: center

Here are some of the available problem types:

-  for incompressible flow:
   **Pb\_\ :math:`\left[\mbox{\textcolor{magenta}{Thermo}}\right]`\ hydraulique\ :math:`\left[\mbox{\textcolor{darkblue}{\_Concentration}}\right]\hspace{-0.15cm}\left[\mbox{\textcolor{Greeen}{\_Turbulent}}\right]`**,

-  for quasi-compressible flow:
   **Pb_Thermohydraulique\ :math:`\left[\mbox{\textcolor{Greeen}{\_Turbulent}}\right]`\ \_QC**,

-  for weakly compressible flow:
   **Pb_Thermohydraulique\ :math:`\left[\mbox{\textcolor{Greeen}{\_Turbulent}}\right]`\ \_WC**,

-  for solid: **Pb_Conduction**,

-  you can find all `problem
   types <TRUST_Reference_Manual.pdf#Pbbase>`__ in the Reference Manual.

where:

-  **hydraulique**: means that we will solve Navier-Stokes equations
   without energy equation,

-  **Thermo**: means that we will solve Navier-Stokes equations with
   energy equation,

-  **Concentration**: that we will solve multiple constituent
   transportation equations,

-  **Turbulent**: that we will simulate a turbulent flow and specify a
   turbulent model (RANS or LES). (Since version v1.8.0, Turbulence
   models are in TrioCFD and not anymore in TRUST).

-  **Conduction**: resolution of the heat equation,

-  **QC**: Navier Stokes equations with energy equation for
   quasi-compressible fluid under low Mach approach,

-  **WC**: Navier Stokes equations with energy equation for weakly
   compressible fluid under low Mach approach.

Domain definition
-----------------

To define the domain, you must name it. This is done thanks to the
following block:

.. container:: center

Then you must add your mesh to your simulation.

.. _Mesh:

Mesh
----

Notice the presence of the tags:

| ``#``\ :raw-latex:`\ `\ ``BEGIN``\ :raw-latex:`\ `\ ``MESH``\ :raw-latex:`\ `\ ``#``
| ``...``
| ``#``\ :raw-latex:`\ `\ ``END``\ :raw-latex:`\ `\ ``MESH``\ :raw-latex:`\ `\ ``#``

in the data file of section `1.3.1 <#data>`__. This is useful for
parallel calculation (see section `7 <#parallel>`__) if well placed in
datafile.

Allowed meshes
~~~~~~~~~~~~~~

**TRUST** allows:

-  quadrangular or triangular undeformed meshing for 2D cases (Figure
   `[2D_mesh] <#2D_mesh>`__),

   .. container:: center

-  hexahedral or tetrahedral undeformed meshing for 3D cases (Figure
   `[3D_mesh] <#3D_mesh>`__).

   .. container:: center

Non standard and hybrid meshing are partially supported thanks to
PolyMAC discretization! (cf Figure `[hybr] <#hybr>`__)

.. container:: center

Import a mesh file
~~~~~~~~~~~~~~~~~~

If your mesh was generated with an external tool like
`Salomé <http://www.salome-platform.org>`__ (open source software),
`ICEM <http://resource.ansys.com/Products/Other+Products/ANSYS+ICEM+CFD>`__
(commercial software), `Gmsh <http://gmsh.info/>`__ (open source
software, included in **TRUST** package) or
`Cast3M <http://www-cast3m.cea.fr/>`__ (CEA software), then you must use
one of the following keywords into your data file:

-  `Read_MED <TRUST_Reference_Manual.pdf#readmed>`__ for a MED file from
   `Salomé <http://www.salome-platform.org>`__,
   `Gmsh <http://gmsh.info/>`__,... ,

-  `Read_File <TRUST_Reference_Manual.pdf#readfile>`__ for a binary mesh
   file from
   `ICEM <http://resource.ansys.com/Products/Other+Products/ANSYS+ICEM+CFD>`__,

-  for another format, see the `Project Reference
   Manual <TRUST_Reference_Manual.pdf#read>`__.

If you want to learn how to build a mesh with Salomé or Gmsh and read it
with **TRUST**, you can look at the exercises of the **TRUST** tutorial:
`here <TRUST_tutorial.pdf#salome>`__ for Salomé and
`here <TRUST_tutorial.pdf#gmsh>`__ for Gmsh.

Quickly create a mesh
~~~~~~~~~~~~~~~~~~~~~

Here is an example of a simple geometry (of non complex channel type)
using the internal tool of **TRUST**:

.. container:: center

| To use this mesh in your data file, you just have to add the previous
  block in your data file or save it in a file named for example
  "*my_mesh.geo*" and add the line:

.. container:: center

| *Do not forget the semicolon at the end of the line!*

Transform mesh within the data file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can also make transformations on your mesh after the **"Mailler"**
or **"Read_"** command, using the following keywords:

-  `Trianguler <TRUST_Reference_Manual.pdf#triangulate>`__ to
   triangulate your 2D cells and create an unstructured mesh.

-  `Tetraedriser <TRUST_Reference_Manual.pdf#tetraedriser>`__ to
   tetrahedralise 3D cells and create an unstructured mesh.

-  `Raffiner_anisotrope <TRUST_Reference_Manual.pdf#raffineranisotrope>`__/`Raffiner_isotrope <TRUST_Reference_Manual.pdf#raffinerisotrope>`__
   to triangulate/tetrahedralise elements of an untructured mesh.

-  `ExtrudeBord <TRUST_Reference_Manual.pdf#extrudebord>`__ to generate
   an extruded mesh from a boundary of a tetrahedral or an hexahedral
   mesh. **Note** that ExtrudeBord in VEF generates 3 or 14 tetrahedra
   from extruded prisms.

-  `RegroupeBord <TRUST_Reference_Manual.pdf#regroupebord>`__ to build a
   new boundary with several boundaries of the domain.

-  `Transformer <TRUST_Reference_Manual.pdf#transformer>`__ to transform
   the coordinates of the geometry.

-  for other commands, see the section
   `interprete <TRUST_Reference_Manual.pdf#interprete>`__ of the Project
   Reference Manual.

**Note** that theses mesh modifications work on all mesh types (i.e.
also for **\*.geo** or **\*.bin** or **\*.med** files).

Test your mesh
~~~~~~~~~~~~~~

| The keyword
  `Discretiser_domaine <TRUST_Reference_Manual.pdf#discretiserdomaine>`__
  is useful to discretize the domain (faces will be created) without
  defining a problem. Indeed, you can create a minimal data file,
  post-process your mesh in lata format (for example) and visualize it
  with VisIt.
| **Note** that you must name all the boundaries!
| Here is an example of this kind of data file:

.. container:: center

To use it, launch in a bash terminal:

::

   # if not already done
   > source $my_path_to_TRUST_installation/env_TRUST.sh
   # then
   > trust my_data_file
   > visit -o file.lata &

| To see how to use VisIt, look at the first **TRUST** tutorial
  exercise: `Flow around an obstacle <TRUST_tutorial.pdf#exo1>`__.
| If you want to learn how to make a mesh with Salomé or Gmsh and read
  it with **TRUST**, you can look at the exercises of the **TRUST**
  tutorial: `here <TRUST_tutorial.pdf#salome>`__ for Salomé and
  `here <TRUST_tutorial.pdf#gmsh>`__ for Gmsh.

Discretization
--------------

| You have to specify the discretization type which can be
  `VDF <TRUST_Reference_Manual.pdf#vdf>`__,
  `EF <TRUST_Reference_Manual.pdf#ef>`__ or
  `VEFPreP1B <TRUST_Reference_Manual.pdf#vefprep1b>`__.
| In **VDF** discretization, the locations of the unknowns are drawn in
  the Figure `[fig_VDF] <#fig_VDF>`__.

.. container:: center

| For **VEFPreP1B**, the locations of the unknowns are drawn in the
  Figure `[fig_VEF] <#fig_VEF>`__.

.. container:: center

In 3D for the pressure, we can also use the P0+P1+Pa discretization for
flow with a strong source term and a low velocity field. In this case
P0+P1 pressure gradient has trouble to match the source term so we use
P0+P1+Pa discretization (cf Figure
`[fig_VEF_pressure_loc] <#fig_VEF_pressure_loc>`__).

.. container:: center

To specify the wanted discretization, you have to add the following
block to your data file:

.. container:: center

You can add parameters to your discretization with the optional keyword
`Read <TRUST_Reference_Manual.pdf#read>`__ (see `VEFPreP1B
discretization <TRUST_Reference_Manual.pdf#vefprep1b>`__).

On the `TrioCFD website <http://triocfd.cea.fr/>`__, you can find
information about:

-  **VDF** discretization in the `PhD thesis of A.
   Chatelain <http://triocfd.cea.fr/Documents/DOCS THESES/these_chatelain_2004.pdf>`__,

-  **VEFPreP1B** discretization (Crouzet-Raviart elements) in the `PhD
   thesis of T.
   Fortin <http://triocfd.cea.fr/Documents/DOCS THESES/these_fortin_2006.pdf>`__
   and `PhD thesis of S.
   Heib <http://triocfd.cea.fr/Documents/DOCS THESES/These_Heib_2003.pdf>`__.

Time schemes
------------

Now you can choose your time scheme to solve your problem. For this you
must specify the time scheme type wanted and give it a name. then you
have to specify its parameters by filling the associated **"Read"**
block.

.. container:: center

Some available time schemes
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here are some available types of explicit schemes:

-  `Scheme_Euler_explicit <TRUST_Reference_Manual.pdf#eulerscheme>`__,

-  `Schema_Adams_Bashforth_order_2 <TRUST_Reference_Manual.pdf#schemaadamsbashforthorder2>`__,

-  `Runge_Kutta_ordre_3 <TRUST_Reference_Manual.pdf#rungekuttaordre3>`__,

And also some available types of implicit schemes:

-  `Scheme_Euler_implicit <TRUST_Reference_Manual.pdf#schemaeulerimplicite>`__,

-  `Schema_Adams_Moulton_order_3 <TRUST_Reference_Manual.pdf#schemaadamsmoultonorder3>`__.

| For other schemes, see `this
  section <TRUST_Reference_Manual.pdf#schematempsbase>`__ of the
  Reference Manual.
| **Note** that you can use semi-implicit schemes activating the
  **diffusion_implicite** keyword in your explicit time scheme.

Calculation stopping condition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You must specify at least one stopping condition for you simulation. It
can be:

-  the final time: **tmax**

-  the maximal allowed cpu time: **tcpumax**

-  the number of time step: **nb_pas_dt_max**

-  the convergency treshold: **seuil_statio**

| **Note** that if the time step reaches the minimal time step
  **dt_min**, **TRUST** will stop the calculation.
| If you want to stop properly your running calculation (i.e. with all
  saves), you may use the *my_data_file*.stop file (cf section
  `5.2 <#stopfile>`__). When the simulation is running, you can see the
  "**0**" value in that file.
| To stop it, put a "**1**" instead of the "**0**", save the file and at
  the next iteration the calculation will stop properly.
| When you don’t change anything in that file, at the end of the
  calculation, you can see that it is writen "**Finished correctly**".

Medium/Type of fluide
---------------------

To specify the medium or fluid, you must add the following block.

.. container:: center

***Fluid_type*** can be one of the following:

-  `Fluide_incompressible <TRUST_Reference_Manual.pdf#fluideincompressible>`__

-  `Fluide_quasi_compressible <TRUST_Reference_Manual.pdf#fluidequasicompressible>`__

-  Fluide_Weakly_Compressible

-  `Solide <TRUST_Reference_Manual.pdf#solide>`__

-  for other types and more information see `Project Reference
   Manual <TRUST_Reference_Manual.pdf#milieubase>`__.

| Since TRUST v1.9.1, the medium should be read in the begining of the
  problem definition (before equations). If you want to solve a coupled
  problem, each medium should be read in the corresponding problem.

Add gravity
-----------

If needed, you can add a gravity term to your simulation. This is done
by adding a uniform field, in the medium block since V1.9.1. For example
in 2D:

.. container:: center

Objects association and discretization
--------------------------------------

Association
~~~~~~~~~~~

Until now, we have created some objects, now we must associate them
together. For this, we must use the
`Associate <TRUST_Reference_Manual.pdf#associate>`__ interpretor:

.. container:: center

.. _discretization-1:

Discretization
~~~~~~~~~~~~~~

Then you must discretize your domain using the
`Discretize <TRUST_Reference_Manual.pdf#discretize>`__ interpretor:

.. container:: center

| The problem *my_problem* is discretized according to the
  *my_discretization* discretization.
| IMPORTANT: A number of objects must be already associated (a domain,
  time scheme, ...) prior to invoking the **Discretize** keyword.
| **Note** that when the discretization step succeeds, the mesh is
  validated by the code.