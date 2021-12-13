.. caldb.rst

.. include:: DRAGONSlinks.txt

.. before we are too tired, let's tackle this one.

.. _basic1_caldb:

*************************
Local calibration manager
*************************

The local calibration manager is the tool that will allow DRAGONS to find the
most appropriate processed calibration for the data being reduced.

The local calibration manager contains all the same rules as the Gemini
Observatory Archive, but works locally, with your files, without internet, and
alongside DRAGONS to make the calibration association as seamless as possible.
As we have seen in the demo earlier, we create the master calibrations, we add
them to the database, and when DRAGONS (``reduce``) needs one, it gets matched
and retrieved automatically.

The system uses a lightweight ``sqlite`` database that will **not** store the
data file, it will store only information **about** the file, like its
location on disk, and key astrodata tags and descriptors.

.. warning::  If you move or delete a processed calibration on disk after
   having entered it in the database, the calibration manager will no longer
   be able to find it.  The database does not contain the file, just information
   about the file.

See the |caldb| documentation for more information, including information about
the programming interface.


Configuration and initialization
================================

.. warning:: The information provided here applies to DRAGONS
   version 3.0 (and older).  The configuration and features of the calibration
   manager in the upcoming version 3.1 are substantially different.

Configuration
-------------
The behavior and configuration of the local calibration manager are controlled
in the file ``~/.geminidr/rsys.cfg``.   When you first install DRAGONS, you
will have to create that directory and that file.

To complete the exercises, your ``rsys.cfg`` file must look like this:

::

    [calibs]
    standalone = True
    database_dir = <where_the_data_package_is>/niriimg_tutorial/playground

The ``standalone`` value should always be ``True``.  At Gemini, we do have
some systems that requires ``False`` but that will never be the case for
normal users.

The value of ``database_dir`` can be any **existing** path on your machine.  The
calibration database will be created, and then expected to be in that directory.
The database name is **always** ``cal_manager.db``. If you change the value
of ``database_dir`` and initialize, it will create and use a different
``cal_manager.db`` in that new path.  You can have as many ``cal_manager.db``
as you want, which one is used is controlled by ``database_dir``.

In other words, you cannot set the name of the database but you can set its
location on disk.

We recommend setting up a new database for each observing project.  It helps
keep things clean and avoids calibrations from other programs being unexpectedly
picked up.  (If they were picked up, they would be a match, but it might get
confusing if you are expecting another calibration to be picked up.)

Here is an example of a ``rsys.cfg`` with several ``database_dir`` entries.
Note that at all times, only one is active.

::

    [calibs]
    standalone = True

    database_dir = /Users/klabrie/data/tutorials/niriimg_tutorial/playground
    #database_dir = /Users/klabrie/data/tutorials/gmosimg_tutorial/playground

    #database_dir = /Users/klabrie/data/workspace/GMOS540feature

Initialization
--------------

Once your ``rsys.cfg`` is set up, you can create a **new** database with

::

    caldb init

If the database already exists, that command will refuse to work.  This ensures
that you will not wipe the database accidentally.  **The** ``init`` **is required
only once.**  If you already have the database created, maybe even populated,
you can switch back to it simply by ensuring ``database_dir`` points to it.

.. _basic1_ex_caldb1:

.. admonition:: Exercise - Caldb 1

    Create a new database in the ``niri_tutorial`` directory.  Keep the
    ``playground`` entry that should already be there, but deactivate it.

    Confirm with ``ls`` that a new ``cal_manager.db`` has been created.  You
    can also use ``caldb config`` to confirm that the new database is active.

    [:ref:`Solution <basic1_solution_caldb1>`]


Usage
=====

The usage is fairly basic.  One can ``list`` the content, ``add`` content,
and ``remove`` content.   DRAGONS will retrieve content from the database.
In DRAGONS version 3.0 and older, DRAGONS cannot add content automatically,
the user needs to add the processed calibrations to the database.

To verify which database is currently active, use the ``config`` option::

    caldb config

To see the content of the database::

    caldb list

To add information about a processed calibration to the database::

    caldb add name_of_file.fits

And finally, to delete information about one file from the database::

    caldb remove name_of_file.fits

``remove`` will only remove the entry in the database, it will not remove
the file on disk.

.. _basic1_ex_caldb2:

.. admonition:: Exercise - Caldb 2

    Do :ref:`Exercise - Caldb 1 <ex_caldb1>` first.

    #. Add the flat field from the demo (``N20160102S0373_flat.fits``) to the
       new calibration manager created in the first exercise.
    #. Show the content of that database is indeed just that file.
    #. Reactivate the original database (the one in ``playground`` that we used
       for the demo) and list its content.  Both the dark and the flat should
       now be listed.

    [:ref:`Solution <basic1_solution_caldb2>`]

