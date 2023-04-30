Parallel calculation
====================

**TRUST** is a plateform which allows to make parallel calculation
following some basic rules:

-  **S**\ ingle **P**\ rogram, **M**\ ultiple **D**\ ata model: tasks
   are split up and run simultaneously on multiple processors with
   different input in order to obtain results faster,

-  messages exchange by **M**\ essage **P**\ assing **I**\ nterface,

-  from a PC to a massively parallel computer, with shared or
   distributed memory.

Basic notions
-------------

To make a parallel calculation, you have to partition your domain. Each
sub-domain will be treated by one processor. In order to have good
performances, ensure you that:

-  sub-domains have almost the same number of cells,

-  joint lengths (boundaries between sub-domains) are minimal,

Performances
------------

| You have to choose a number of processors which is in agreement with
  the number of cells. So, you can evaluate your speed-up (sequential
  time/parallel time which must be as close as possible of the number of
  processors) or efficiency (=1/SpeedUp) to choose the right number of
  processors.
| From our experience, for good performance with **TRUST**, each
  processor has to treat between 20000 and 30000 cells.

Partitioning
------------

To run a parallel calculation, you must:

-  make some modifications on your *my_data_file.data* file,

-  do two runs:

   -  the first one, to partitioning and create your ’n’ sub-domains
      (two methods will by presented),

   -  the second one, to read your ’n’ sub-domains and run the
      calculation on ’n’ processors.

.. _decjdd:

The different blocks
~~~~~~~~~~~~~~~~~~~~

Different blocks appear in the data file.

-  | **Modifications on the mesh block**
   | First you may add the tags "# BEGIN MESH #" and "# END MESH #"
     before and after your mesh block, for example:

   .. container:: center

   You can refer to section `3.3 <#Mesh>`__ to have more information.

-  | **Adding a partitioning block**
   | You may now add the partitioning block which contains the cutting
     instruction, after your mesh block:

   .. container:: center

   | Where *partitioner_name* is the name of the chosen partitioner, one
     of **METIS**, **Tranche**, **Sous_Zones**, **Partition** or
     **Fichier_Decoupage** (cf section `7.3.3 <#partitioner>`__).
   | The **"Larg_joint"** keyword allows to specify the overlapping
     width value. You can see the documentation of `this part in the
     Project Reference Manual <TRUST_Reference_Manual.pdf#partition>`__.
   | **Note** the "End" before the last line. It will be useful for the
     cutting step.
   | This block will make the partitioning of your domain into the
     specified number of sub-domains during the partitioning run.

-  | **Adding a block to read the sub-domains**
   | At last, you will add a block which will be activated during the
     parallel calculation and will allow to read the sub-domains:

   .. container:: center

.. _makePARdata:

Partitionning: "Assisted" method
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Here we will use the "trust -partition datafile" command line to make
  the partitioning step. We consider that you have correctly add the "#"
  in your *my_data_file.data* file with the partitioning block and
  cutting block.
| **Be careful** with the hashtags "#", they are interpreted by the
  script!
| To automatically perform the partitioning step and obtain the parallel
  data file, you have to run:

::

   > trust -partition my_data_file [parts_number]

| **Note** that here parts_number is the number of sub-domains created
  but it is also the number of processors which will be used.
| This command creates:

-  a *SEQ_my_data_file.data* file which is a backup file of
   *my_data_file.data* the sequential data file,

-  | a *DEC_my_data_file.data* file which is the first data file to be
     run to make the partitioning. It is immediately run by the command
     line **trust -partition datafile** to create a partition, located
     in the *DOM_000n*\ **.Zones** files.
   | **Note** that the code stops reading this file at the **"End"**
     keyword just before the "# END PARTITION #" block.

-  | a *PAR_my_data_file.data* file which is the data file for the
     parallel calculation. It reads the *DOM_000n*\ **.Zones** files
     through the instruction **"Scatter"**.
   | **Note** that the meshing and cut of the mesh are commented here.

To see your sub-domains, you can run:

``>``\ :raw-latex:`\ `\ ``trust``\ :raw-latex:`\ `\ ``-mesh``\ :raw-latex:`\ `\ ``PAR_my_data_file``

For more information, you can go to see this
`exercise <TRUST_tutorial.pdf#exo_para_2>`__ in the **TRUST** tutorial.

.. _partitioner:

**TRUST** available partitioning tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In **TRUST**, you can make partitioning with:

-  the external partitionning library
   `"METIS" <http://glaros.dtc.umn.edu/gkhome/views/metis>`__ (open
   source). It is a general algorithm that will generate a partition of
   the domain (cf `Project Reference
   Manual <TRUST_Reference_Manual.pdf#partitionneurmetis>`__),

   .. container:: center

-  internal **TRUST** partitioning tool:
   `Tranche <TRUST_Reference_Manual.pdf#partitionneurtranche>`__ which
   makes parts by cutting the domain following x, y and/or z directions.

   .. container:: center

-  `other internal partitioning
   tools <TRUST_Reference_Manual.pdf#partitionneurderiv>`__.

The Figure `7.2 <#partitioning>`__ is an example of what you can obtain
by cutting a 1m x 1m square, divided in three parts using
`METIS <http://glaros.dtc.umn.edu/gkhome/views/metis>`__ and the same
square divided in three slices following the x direction with
**Tranche**.

.. container:: centering

   ==================== ========
   |Partitioning tools|  |image1|
   METIS                 Tranche
   ==================== ========

You can see the documentation of `this part in the Project Reference
Manual <TRUST_Reference_Manual.pdf#partition>`__.

Overlapping width value
~~~~~~~~~~~~~~~~~~~~~~~

To make the partitioning, you will have to specify the *overlapping
width value*. This value corresponds to the thickness of the virtual
ghost zone (data known by one processor though not owned by it) i.e. the
number of vertices or elements on the remote sub-domain known by the
local sub-domain (cf Figure `7.3 <#overlap>`__).

.. container:: center

   .. figure:: overlap.jpeg
      :alt: Overlapping width
      :name: overlap
      :width: 96.0%

      Overlapping width

This value depends on the space discretization scheme orders:

-  1 if 1st or 2nd order,

-  2 if 3rd or 4th order.

**Note** that in general, you will use "2"!

Running a parallel calculation
------------------------------

On a PC
~~~~~~~

To launch the calculation, you have to run the calculation by the usual
command completed by the number of processors needed:

::

   > trust my_parallel_data_file procs_number

| and *procs_number* is the number of processors used. In fact it is the
  same as the number of sub-domains.
| You can see the **TRUST**\ & **TrioCFD** user slides in the "Parallel
  calculation" section for more information and those two exercises of
  the **TRUST** tutorial: `exercise 1 <TRUST_tutorial.pdf#exo_para_1>`__
  and `exercise 2 <TRUST_tutorial.pdf#prm_para>`__.

On a cluster
~~~~~~~~~~~~

You must submit your job in a queue system. For this, you must have a
submission file. **TRUST** can create a submission file for you **on
clusters on which the support team has done installations**. To create
this file, run:

::

   > trust -create_sub_file my_parallel_data_file

| You obtain a file named **"sub_file"**, you can open it and
  verify/change values(for example the name of the job, the name of the
  exe, ...).
| Then you must submit you calculation with:

::

   > sbatch sub_file

or

::

   > ccc_msub sub_file

| following the queue system of the cluster.
| You can see the **TRUST**\ & **TrioCFD** user slides in the "Parallel
  calculation" section for more information and `this exercise of the
  TRUST tutorial <TRUST_tutorial.pdf#exo_para_3>`__.

.. _visualization-1:

Visualization
-------------

To visualize your probes, you can use the CurvePlot tool, with the
command line:

::

   > trust -evol my_parallel_data_file

| or use Gnuplot or any software which reads values in columns in a
  file.
| There are three ways to visualize your parallel results with VisIt:

-  HPCDrive or Nice DCV on CCRT/TGCC clusters: opens a deported graphic
   session on dedicated nodes with more memory (on TGCC cluster:
   `HPCDrive <https://visu-tgcc.ccc.cea.fr/HPCDrive/home>`__),

-  local mode: copy your results from the cluster to your local computer
   and open it with a local parallel version of VisIt with:

   ::

      > visit -np 4 &

You can have a look at the **TRUST**\ & **TrioCFD** user slides in the
"Parallel calculation description" section.

Useful information
------------------

Modify the mesh
~~~~~~~~~~~~~~~

If you want to modify your mesh, you have two possibilities:

-  modify the *my_data_file.data* file and run:

   ::

      > trust -partition my_data_file [parts_number]

   | Be carefull it will erase the *SEQ_my_data_file.data*,
     *DEC_my_data_file.data* and
   | *PAR_my_data_file.data* files and creates new ones.
   | Then it will run the new *DEC_my_data_file.data* file which gives
     your new *DOM_000n*\ **.Zones** files.

-  modify the meshing part of file *DEC_my_data_file.data* and run it
   with:

   ::

      > trust DEC_my_data_file

Then run the parallel calculation normally, on the new
*DOM_000n*\ **.Zones** files.

::

   > trust PAR_my_data_file procs_number

Modify calculation parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to modify the calculation parameters, you can modify:

-  the file *my_data_file.data* and run:

   ::

      > trust -partition data_file_name [parts_number]

   | But it will erase the *SEQ_my_data_file.data*,
     *DEC_my_data_file.data* and
   | *PAR_my_data_file.data* files and create new ones.
   | Then it will run the new *DEC_my_data_file.data* file which gives
     your new *DOM_000n*\ **.Zones** files.
   | **Note** that in that case, you don’t need to re-create the mesh so
     you can use the second point below:

-  modify the *PAR_my_data_file.data* file *without* running "trust
   -partition datafile" command line.

Then run the *PAR_my_data_file.data* file with:

::

   > trust PAR_my_data_file procs_number

**Note** that if after a certain time, you want to reopen an old case
and understand want you did in it without any doubts, you may create two
files by hands:

-  one "BuildMeshes.data" file only for the mesh and the cut of the
   mesh, and

-  one "calculation.data" file for the parallel calculation.

You will run it like:

::

   > trust BuildMeshes
   > trust calculation processors_number

For this point, you can have a look at `this exercise of the TRUST
tutorial <TRUST_tutorial.pdf#prm_para>`__.

.. [1]
   The post-processed pressure is the pressure divided by the fluid’s
   density (:math:`P/\rho+gz`) on incompressible laminar calculation.
   For turbulent, pressure is :math:`P/\rho+gz+2/3*k` cause the
   turbulent kinetic energy is in the pressure gradient.

.. [2]
   Tref indicates the value of a reference temperature and must be
   specified by the user. For example, H_echange_293 is the keyword to
   use for Tref=293K.

.. [3]
   distance_paroi is a field which can be used only if the mixing length
   model (see 2.15.1.2) is used in the data file.

.. [4]
   The post-processed pressure is the pressure divided by the fluid’s
   density (:math:`P/\rho+gz`) on incompressible laminar calculation.
   For turbulent, pressure is :math:`P/\rho+gz+2/3*k` cause the
   turbulent kinetic energy is in the pressure gradient.

.. [5]
   Tref indicates the value of a reference temperature and must be
   specified by the user. For example, H_echange_293 is the keyword to
   use for Tref=293K.

.. [6]
   distance_paroi is a field which can be used only if the mixing length
   model (see 2.15.1.2) is used in the data file.

.. [7]
   The post-processed pressure is the pressure divided by the fluid’s
   density (:math:`P/\rho+gz`) on incompressible laminar calculation.
   For turbulent, pressure is :math:`P/\rho+gz+2/3*k` cause the
   turbulent kinetic energy is in the pressure gradient.

.. [8]
   Tref indicates the value of a reference temperature and must be
   specified by the user. For example, H_echange_293 is the keyword to
   use for Tref=293K.

.. [9]
   distance_paroi is a field which can be used only if the mixing length
   model (see 2.15.1.2) is used in the data file.

.. |Partitioning tools| image:: partition_metis.jpeg
   :name: partitioning
   :width: 45.0%
.. |image1| image:: partition_tranche.jpeg
   :name: partitioning
   :width: 45.0%