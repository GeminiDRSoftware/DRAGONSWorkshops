.. caldb.rst

.. include:: DRAGONSlinks.txt

.. _caldb:

*************************
Local calibration manager
*************************

.. before we are too tired, let's tackle this one.

what does it do, what is it for?

The local calibration manager uses the same calibration association rules as
the Gemini Observatory Archive, but operates locally on your computer and does
not require internet.

The system uses a lightweight ``sqlite`` database that will **not** store the
data file, it will store only information **about** the file, like its
location on disk, and key astrodata tags and descriptors.

.. warning:: The information provided here applies to DRAGONS
   version 2.x.  The configuration and features of the calibration manager
   in the upcoming version 3.x is substantially different.

configuration and initialization
recommend to set up new db for each program to avoid calibrations from other
programs being accidentally picked up.

show how to add, list, remove.

.. warning::  If you move or delete a processed calibration on disk after
   having entered it in the database, the calibration manager will no longer
   be able to find it.  The database does not contain the file, just information
   about the file.


exercises
* create new database in niri_tutorial (instead of playground).  Keep the
  playground entry, but desactivate it.  Confirm with 'ls' that it's been
  created
* add the flat field from the demo to the new manager, and show that indeed
  the content of the database is just that file.
* reactivate the original database in playground and list its content.