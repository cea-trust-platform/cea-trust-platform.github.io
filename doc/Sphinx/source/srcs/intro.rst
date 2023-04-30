Introduction
============

| This document constitutes the generic guide for **TRUST software** and
  its **Baltik projects**.
| **TRUST** is a thermohydraulic software package for CFD simulations
  for incompressible monophasic flow.
| You can create new project based on **TRUST** plateform. Theses
  projects are named **"BALTIK"** projects.
| The two currently available modules include a VDF calculation module
  "Finite Difference Volume" and a VEF calculation module "Finite
  Element Volume".
| The VDF and VEF validated modules are designed to process the 2D or 3D
  flow of Newtonian, incompressible, weakly expandable fluids the
  density of which is a function of a local temperature and
  concentration values (Boussinesq approximation).

Before TRUST: a modular software named Trio_U
---------------------------------------------

**TRUST** was born from the cutting in two pieces of **Trio_U**
software. **Trio_U** was a software brick based on the **Kernel** brick
(which contains the equations, space discretizations, numerical schemes,
parallelism...) and used by other CEA applications (cf Figure
`[TrioU] <#TrioU>`__).

.. container:: center

| We could create new projects based on Kernel brick or **Trio_U**
  brick. Theses projects were named **"BALTIK"** projects: "**B**\ uild
  an **A**\ pplication **L**\ inked to **T**\ r\ **I**\ o_U
  **K**\ ernel".
| In 2015, **Trio_U** was divided in two parts: **TRUST** and
  **TrioCFD**.

-  **TRUST** is a new platform, its name means: "**TR**\ io\_\ **U**
   **S**\ oftware for **T**\ hermohydraulics",

-  **TrioCFD** is a BALTIK project based on **TRUST**, which contains
   the following models: FT, Radiation, LES, zoom...

Here is the structure of **TRUST** platform (cf Figure
`[TRUST] <#TRUST>`__):

.. container:: center

**Note** that: **Trio_U = TRUST + TrioCFD**.

Short history
-------------

**TRUST** is developed at the CEA/DES/ISAS/DM2S/STMF service. The
project starts in 1994 and improved versions were built ever since:

-  1994: start of the project Trio_U

-  01/1997: v1.0 (VDF only)

-  06/1998: v1.1 (VEF version)

-  04/2000: v1.2 (parallel version)

-  07/2001: v1.3 (radiation model)

-  11/2002: v1.4 (new LES turbulence models)

-  02/2006: v1.5 (VDF/VEF Front Tracking)

-  10/2009: v1.6 (data structure revamped)

-  06/2015: v1.7 (cut into **TRUST** and **TrioCFD** + switch to open
   source)

-  11/2019: v1.8 (Turbulence features are moved from **TRUST** to
   **TrioCFD** + PolyMAC discretization)

-  06/2022: v1.9 (Pb_Multiphase in **TRUST**\ + PolyMAC V2
   discretization + Pb_HEM in **TrioCFD**)

Data file
---------

| To launch a calculation with **TRUST**, you need to write a "data
  file" which is an input file for **TRUST** and will contain all the
  information about your simulation. Data files are written following
  some rules as shown below. But their language is not a programming
  language, users can’t make loops or switch...
| **Note** that:

-  lines between # ... # and /\* ... \*/ are comments,

-  | in that document, words in **bold** are **TRUST** keywords, you can
     highlight them in your file editor with the command line (details
     in section `1.4 <#Run>`__):
   | ``> trust -config nedit|vim|emacs``

-  braces "{ }" are elements that **TRUST** reads and interprets, so
   don’t forget them and *put space* *before and after them*,

-  elements between bracket "[ ]" are optional.

.. _data:

Data file example: base blocks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Here is the template of a basic sequential data file:

Basic rules
~~~~~~~~~~~

| There is no line concept in **TRUST**.
| Data files uses *blocks*. They may be defined using the braces:

.. container:: center

Objects notion
~~~~~~~~~~~~~~

**Objects** are created in the data set as follows:

.. container:: center

-  **export**: if this keyword is included, *identificateur*
   (identifier) will have a global range, if not, its range will be
   applied to the block only (the associated object will be destroyed on
   exiting the block).

-  *Type*: must be a type of object recognised by **TRUST**, correspond
   to the C++ classes. The list of recognised types is given in the file
   hierarchie.dump.

-  *identificateur*: the identifier of the object type *Type* created,
   correspond to an instancy of the C++ class *Type*. **TRUST** exits in
   error if the identifier has already been used.

There are several object types. Physical objects, for example:

-  A **Fluide_incompressible** (incompressible_Fluid) object. This type
   of object is defined by its physical characteristics (its dynamic
   viscosity :math:`\mu` (keyword **mu**), its density :math:`\rho`
   (keyword **rho**), etc...),

-  A **Domaine**.

More abstract object types also exist:

-  A **VDF** or **VEF** according to the discretization type,

-  A **Scheme_euler_explicit** to indicate the time scheme type,

-  A **Solveur_pression** to denote the pressure system solver type,

-  A **Uniform_field** to define, for example, the gravity field.

Interpretor notion
~~~~~~~~~~~~~~~~~~

**Interprete** (interpretor) type objects are then used to handle the
created objects with the following syntax:

.. container:: center

-  | *Type_interprete*: any type derived from the **Interprete**
     (Interpretor) type recognised by **TRUST**. In this manual, they
     are written in **bold**. You can highlight them in your file editor
     with the command (details in section `1.4 <#Run>`__):
   | ``> trust -config nedit|vim|emacs``

-  *argument*: an argument may comprise one or several object
   identifiers and/or one or several data blocks.

| Interpretors allow some operations to be carried out on objects.
| Currently available general interpretors include **Read**,
  **Read_file**, **Ecrire** (Write), **Ecrire_fichier** (Write_file),
  **Associate**.

Example
~~~~~~~

A data set to write Ok on screen:

.. container:: center

Important remarks
~~~~~~~~~~~~~~~~~

#. To insert *comments* in the data set, use # .. # (or /\* ... \*/),
   the character # must always be enclosed by blanks.

#. The comma separates items in a list (a comma must be enclosed with
   spaces or a new line).

#. Interpretor keywords are recognised indiscriminately whether they are
   written in lower and/or upper case.

#. **On the contrary, object names (identifiers) are recognised
   differently if they are written in upper or lower case.**

#. In the following description, items (keywords or values) enclosed by
   [ and ] are *optional*.

.. _Run:

Running a data file
-------------------

To use **TRUST**, your shell must be "bash". So ensure you are in the
right shell:

::

   > echo $0
   /bin/bash

To run your data file, you must initialize the TRUST environment using
the following command:

::

   > source $my_path_to_TRUST_installation/env_TRUST.sh
   TRUST vX.Y.Z support : trust@cea.fr
   Loading personal configuration /$path_to_my_home_directory/.perso_TRUST.env

Sequential calculation
~~~~~~~~~~~~~~~~~~~~~~

You can run your sequential calculation:

::

   > cd $my_test_directory
   > trust [-evol] my_data_file

where "trust" command call the "trust" script. You can have the list of
its options with:

::

   > trust -help

or

::

   > trust -h

Here is a panel of available options:

::

   Usage: trust [option] datafile [nb_cpus] [1>file.out] [2>file.err]
   Where option may be:
   -help|-h                      : List options.
   -baltik [baltik_name]         : Instanciate an empty Baltik project.
   -index                        : Access to the TRUST ressource index.
   -doc                          : Access to the TRUST manual (Generic Guide).
   -html                         : Access to the doxygen documentation.
   -config nedit|vim|emacs|gedit : Configure nedit or vim or emacs or gedit with TRUST keywords.
   -edit                         : Edit datafile.
   -xcheck                       : Check the datafile's keywords with xdata.
   -xdata                        : Check and run the datafile's keywords with xdata.
   -partition                    : Partition the mesh to prepare a parallel calculation (Creation of the .Zones files).
   -mesh                         : Visualize the mesh(es) contained in the data file.
   -eclipse-trust                : Generate Eclipse configuration files to import TRUST sources.
   -eclipse-baltik               : Generate Eclipse configuration files to import BALTIK sources (TRUST project should have been configured under Eclipse).
   -probes                       : Monitor the TRUST calculation only.
   -evol                         : Monitor the TRUST calculation (GUI).
   -prm                          : Write a prm file (deprecated).
   -jupyter                      : Create basic jupyter notebook file.
   -clean                        : Clean the current directory from all the generated files by TRUST.
   -search keywords              : Know the list of test cases from the data bases which contain keywords.
   -copy                         : Copy the test case datafile from the TRUST database under the present directory.
   -check|-ctest all|testcase|list            : Check|ctest the non regression of all the test cases or a single test case or a list of tests cases specified in a file.
   -check|-ctest function|class|class::method : Check|ctest the non regression of a list of tests cases covering a function, a class or a class method.
   -gdb                          : Run under gdb debugger.
   -valgrind                     : Run under valgrind.
   -valgrind_strict              : Run under valgrind with no suppressions.
   -callgrind                    : Run callgrind tool (profiling) from valgrind.
   -massif                       : Run massif tool (heap profile) from valgrind.
   -heaptrack                    : Run heaptrack (heap profile). Better than massif.
   -advisor                      : Run advisor tool (vectorization).
   -vtune                        : Run vtune tool (profiling).
   -perf                         : Run perf tool (profiling).
   -trace                        : Run traceanalyzer tool (MPI profiling).
   -create_sub_file              : Create a submission file only.
   -prod                         : Create a submission file and submit the job on the main production class with exclusive resource.
   -bigmem                       : Create a submission file and submit the job on the big memory production class.
   -queue queue                  : Create a submission file with the specified queue and submit the job.
   -c ncpus                      : Use ncpus CPUs allocated per task for a parallel calculation.
   datafile -help_trust          : Print options of TRUST_EXECUTABLE [CASE[.data]] [options].
   -convert_data datafile        : Convert a data file to the new 1.9.1 syntax (milieu, interfaces, read_med and champ_fonc_med).

Parallel calculation
~~~~~~~~~~~~~~~~~~~~

To run a parallel calculation, you must do two runs:

-  the first one, to partition and create your ’n’ sub-domains (two
   methods: "By hand" method below and "Assisted" method cf parts
   `7.3.1 <#decjdd>`__ & `7.3.2 <#makePARdata>`__),

-  the second one, to read your ’n’ sub-domains and run the calculation
   on ’n’ processors.

We will explain here how to do such work:

-  | **Partitioning: "By hand" method**
   | You have to make two data files:

   -  *BuildMeshes.data* and

   -  *Calculation.data*.

   The *BuildMeshes.data* file only contains the same information as the
   begining of the sequential data file and partitioning information.
   This file will create the sub-domains (cf .Zones files).

   .. container:: center

   Run the *BuildMeshes.data* with **TRUST**:

   ::

      > trust BuildMeshes

   | You may have obtained files named *DOM_000n*\ **.Zones** which
     contains the ’n’ sub-domains.

-  | **Read the sub-domains**
   | The *Calculation.data* file contains the domain definition, the
     block which will read the sub-domains and the problem definition
     (as in sequential calculation).

   .. container:: center

   Run the *Calculation.data* file with **TRUST**:

   ::

      > trust Calculation procs_number

   | This will read your *DOM_000n*\ **.Zones** files. You can see the
     documentation of the **"scatter"** keyword in `this part of the
     Project Reference Manual <TRUST_Reference_Manual.pdf#scatter>`__.

For more information, you can see this `exercise in the TRUST
tutorial <TRUST_tutorial.pdf#exo_para_1>`__.

Visualization
-------------

To learn how to use the "**-evol**" option, you can see the first
exercise of the **TRUST** tutorial: `Flow around an
obstacle <TRUST_tutorial.pdf#exo1>`__.