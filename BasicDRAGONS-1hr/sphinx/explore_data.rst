.. explore_data.rst

.. include:: DRAGONSlinks.txt

.. _explore_data:


************
Explore data
************

Once we have reduced data, we probably want to look at.  In fact,
even before we start the reduction, we should look at the raw data in case
of issues.

DRAGONS is **not** an analysis package.  But it does offer a basic way to
display images.

The ``display`` and ``inspect`` primitives can be used to visually inspect
the data with ``ds9``.  Those are primitives not recipes, yet ``reduce`` can
run individual primitives.

.. note::  ``ds9`` must be launched by the user ahead of running the primitive.

Display
=======

To display an image to ``ds9``::

    reduce -r display N20160102S0271_stack.fits

It is possible to display to different buffer in ``ds9``, eg. to allow
blinking between the frames.

::

    reduce -r display N20160102S0271_stack.fits -p frame=2


Inspect
=======

It is recommended to inspect the raw data prior to reducing the data.  A quick
visual inspection can catch obviously bad data, or flag some frames for
more detailed inspection.  Having to run display on each frame individually
would be painful.  The primitive ``inspect`` should be used here.  It
accepts a list of file and will display each frame in sequence, adding a short
pause between the frames.  The length pause can be controlled with the ``pause``
parameter.

::

    reduce -r inspect @stdstar.lis

::

    reduce -r inspect @stdstar.lis -p pause=1

