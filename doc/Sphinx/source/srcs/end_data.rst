End of the data file
====================

Solve
-----

Now that you have finished to specify all your computation parameters,
you may add the `"Solve" <TRUST_Reference_Manual.pdf#solve>`__ keyword
at the end of your data file, in order to solve your problem. You may
also add the **"End"** keyword to specify the end of your data file.

.. container:: center

| For more details, see the `Project Reference
  Manual <TRUST_Reference_Manual.pdf#solve>`__.
| You can see methods to run your data file in section `1.4 <#Run>`__.

.. _stopfile:

Stop a running calculation
--------------------------

Your calculation will automatically stop if it has reached:

-  the end of the calculation time,

-  the maximal allowed cpu time,

-  the maximal number of iterations or

-  the threshold of convergence.

| You may use the *my_data_file*\ **.stop** file, if you want to stop
  properly your running calculation (i.e. with all saves).
| When the simulation is running, you can see the "**0**" value in that
  file. To stop it, put a "**1**" instead of the "**0**" and at the next
  iteration the calculation will stop properly. When you don’t change
  anything to that file, at the end of the calculation, you can see that
  it is writen "**Finished correctly**".

Save
----

**TRUST** makes automatic backups during the calculation. The unknowns
(velocity, temperature,...) are saved in:

-  one **.xyz** file, happening:

   -  at the end of the calculation,

   -  but, user may disable it with the specific keyword
      "`EcritureLectureSpecial
      0 <TRUST_Reference_Manual.pdf#ecriturelecturespecial>`__" added
      just before the **"Solve"** keyword.

-  one (or several in case of parallel calculation) **.sauv** files,
   happening:

   -  at the start of the calculation,

   -  at the end of the calculation,

   -  each 23 hours of CPU, to change it, uses
      **"periode_sauvegarde_securite_en_heure"** keyword (default value
      23 hours),

   -  user may also specify a time physical period with **"dt_sauv"**
      keyword,

   -  periodically using **"tcpumax"** keyword for which calculation
      stops after the specified time (default value :math:`10^{30}`),
      use it for calculation on CCRT/TGCC and CINES clusters for
      example.

| By default, the name for the **.sauv** files is
  **"filename_problemname.sauv"** for sequential calculation,
  **"filename_problemname_000n.sauv"** for parallel calculation (one per
  process). The format of theses files is binary and the files are
  completed during successive saves.
| You can change the behaviour using the following keywords just before
  the **solve** instruction:

.. container:: center

| with **"xyz"**: the **.xyz** file is written instead of the **.sauv**
  files.
| **Note** that, you can use **"sauvegarde_simple"** instead of
  **"sauvegarde"** where the .sauv or .xyz file is deleted before saves,
  in order to keep disk space:

.. container:: center

| For more details, see the `Project Reference
  Manual <TRUST_Reference_Manual.pdf#Pbbase>`__.

Resume
------

To resume your calculation, you may:

-  change your initial time, the new initial time will be the real final
   calculation time of the previous calculation (cf .err file),

-  change your final calculation time to the new wanted value and

-  add the following block just before the **"Solve"** keyword:

   .. container:: center

You can resume your calculation:

-  from .sauv file(s) (one file per process): you can only resume the
   calculation with the **same number of equations** on **the same
   number of processes**,

-  or from a .xyz file: here you can resume your calculation by
   **changing the number of equations solved** and/or with a **different
   number of processes**.

Instead of **"reprise"** keyword, you can use **"resume_last_time"**
where **tinit** is automatically set to the last time of saved files
(but you may change **tmax**):

.. container:: center

| For examples, see the `TRUST
  tutorial <TRUST_tutorial.pdf#save_restart>`__ and the `Project
  Reference Manual <TRUST_Reference_Manual.pdf#Pbbase>`__.
| **Note** that you can run a calculation with initial condition read
  into a save file (.xyz or .sauv) from a previous calculation using
  `Champ_Fonc_reprise <TRUST_Reference_Manual.pdf#champfoncreprise>`__
  or read a into a MED file with
  `Champ_Fonc_MED <TRUST_Reference_Manual.pdf#champfoncmed>`__.