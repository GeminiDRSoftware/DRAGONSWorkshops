.. demo_imaging.rst

.. include:: DRAGONSlinks.txt

.. _demo_imaging:

************
Demo imaging
************

To get ourselves oriented and get a feel for how data is processed with
DRAGONS, we will run a full reduction on our sample data.  The full tutorial
using this data can be found at ??.   Here we go through the steps with a bit
less discussion than in the full tutorial as we already cover most aspects
in greater details in this workshop through the hands-on exercises.

The observations are of an extended source, a nearby galaxy.  The instrument
used is NIRI.  The science sequence is a series of dithers on target with full
offset to sky.  We will create a master dark, a BPM, a master flat, a reduced
flux standard, and finally create a calibrated, sky-subtracted stack of the
science observations.

Let's run all the steps.

#. Set up the local calibration manager
#. Create the file lists
#. Create the calibration files (dark, bpm, flat)
#. Reduce the flux standard
#. Reduce the science observations

::

    cd <where_the_data_package_is>/niriimg_tutorial/playground


Set up the local calibration manager
====================================
We will discuss the local calibration manager in a later chapter.

The calibration manager is the first thing to set up when starting on a data
reduction project.  It provides the automated calibration association.

The file to pay attention to is: ``~/.geminidr/rsys.cfg`` (DRAGONS v2.x)

Edit that file to contain the following::

    [calibs]
    standalone = True
    database_dir = <where_the_data_package_is>/niriimg_tutorial/playground

About the path for ``database_dir``, we recommend the directory you are using
to work on the data.  But it can be anywhere.

Once the configuration file is set up, initialize the database.  This needs to
be done only once per database.

::

    caldb init

This creates a new file, ``cal_manager.db``, in the directory specified by
``database_dir``.


Create the file lists
=====================
The user has to sort the files.  DRAGONS is a "User in the loop" pipeline.
We have some tools to help.  We will have a closer look at them later.

**Darks**

Long darks that match the science, and short darks that will be used to create
a bad pixel mask.

::

    dataselect ../playdata/*.fits --tags DARK --expr='exposure_time==1' -o darks1s.lis
    dataselect ../playdata/*.fits --tags DARK --expr='exposure_time==20' -o darks20s.lis

**Flats**

The flats are a series of lamp-on and lamp-off flats.

::

    dataselect ../playdata/*.fits --tags FLAT -o flats.lis

**Flux standard**

::

    dataselect ../playdata/*.fits --expr='object=="FS 17"' -o stdstar.lis

**Science**

::

    dataselect ../playdata/*.fits --tags IMAGE --xtags CAL -o target.lis


Create the master dark, the BPM, and the master flat
====================================================

**Dark**

Create the master dark and add it to the calibration manager.

::

    reduce @darks20s.lis
    caldb add N20160102S0423_dark.fits

The ``@`` character before the name of the input file is the "at-file" syntax.
We will look into this later.

**Bad pixel mask**

This uses flats and short darks to identify bad pixels, hot and dead.  We use
a non-default recipe from the NIRI FLAT recipe library.

::

    reduce @flats.lis @darks1s.lis -r makeProcessedBPM

The BPM produced is named ``N20160102S0373_bpm.fits``.

The local calibration manager does not yet support BPMs so we cannot add
it to the database. We have to pass it manually to "|reduce|" to use it, as we
will show below.

**Flat**

Create the master flat from the lamp-on and lamp-off flats and add it to the
calibration database.  We set the ``user_bpm`` input parameter of the primitive
``addDQ`` to the BPM we just created.  We learn more about customization of
input parameters later.

::

    reduce @flats.lis -p addDQ:user_bpm=N20160102S0373_bpm.fits
    caldb add N20160102S0373_flat.fits


Reduce the flux standard
========================
This is short series of on-target dither observations.  Darks are not obtained
for NIRI flux standard so we turn that step off.  The flat will be picked up
from the calibration database.

::

    reduce @stdstar.lis -p addDQ:user_bpm=N20160102S0373_bpm.fits darkCorrect:do_dark=False


Reduce the science observations
===============================
The sequence is a set of dither on target with full offsets to sky.  DRAGONS
will sort them during the sky correction.  All calibrations are picked up
automatically from the calibration database.  Because the target fills the
field of view, we need to turn off the scaling of the sky frames.

::

    reduce @target.lis -p addDQ:user_bpm=N20160102S0373_bpm.fits skyCorrect:scale_sky=False


.. image:: _graphics/extended_before.png
   :scale: 60%
   :align: left

.. image:: _graphics/extended_after.png
   :scale: 60%
   :align: left
