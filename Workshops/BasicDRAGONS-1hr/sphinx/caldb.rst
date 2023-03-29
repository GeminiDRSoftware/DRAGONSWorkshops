.. caldb.rst

.. .. include:: DRAGONSlinks.txt

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

.. warning::
   The information provided here applies to DRAGONS
   version 3.1 (and newer).  The configuration and features of the calibration
   manager in the previous versions, 3.0 and older, are substantially different.
   Please refer to a previous version of this workhop if you are using
   DRAGONS 3.0 or older,

     `<https://collection-of-dragons-workshops.readthedocs.io/en/v1.0.0/BasicDRAGONS-1hr/sphinx/caldb.html>`_

Configuration
-------------
The behavior and configuration of the local calibration manager are controlled
in the file ``~/.dragons/dragonsrc``.   When you first install DRAGONS, you
will have to create that directory and that file.

To complete the exercises, your ``dragonsrc`` file must look like this:

::

    [calibs]
    databases = <where_the_data_package_is>/niriimg_tutorial/playground/cal_manager.db get

The value of ``databases`` can be any **existing** path on your machine.  The
calibration database will be created, and then expected to be in that directory.
The database name can be anything, here we name it ``cal_manager.db`` for
continuity with pre-v3.1 DRAGONS.

We recommend setting up a new database for each observing project.  It helps
keep things clean and avoids calibrations from other programs being unexpectedly
picked up.  (If they were picked up, they would be a match, but it might get
confusing if you are expecting another calibration to be picked up.)

Here is an example of a ``dragonsrc`` with several ``databases`` entries.
Note that at all times, **only one** ``databases`` directive is active, the
others are commented out.

::

    [calibs]
    databases = /Users/klabrie/data/tutorials/niriimg_tutorial/playground/cal_manager.db get
    #databases = /Users/klabrie/data/tutorials/gmosimg_tutorial/playground/cal_manager.db get

    #databases = /Users/klabrie/data/workspace/GMOS540feature/cal_manager.db get

Initialization
--------------

Once your ``dragonsrc`` is set up, you can create a **new** database with

::

    caldb init

If the database already exists, that command will refuse to work.  This ensures
that you will not wipe the database accidentally.  **The** ``init`` **is required
only once.**  If you already have the database created, maybe even populated,
you can switch back to it simply by ensuring ``databases`` points to it.

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
and ``remove`` content.   DRAGONS will retrieve content from the database.  If
the "store" flag is set, DRAGONS will also automatically add content to the
database.

.. warning:: In DRAGONS version 3.0 and older, DRAGONS cannot add content
      automatically, the user **must** add the processed calibrations to the
      database with an explicit ``caldb add``.

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

