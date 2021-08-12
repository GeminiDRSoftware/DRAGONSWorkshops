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

By default, the saturated pixels are marked.  This can be turned off by
setting the ``threshold`` parameter to "None".

It is possible to display to different buffer in ``ds9``, eg. to allow
blinking between the frames.

::

    showpars N20160102S0271_stack.fits display

    Dataset tagged as {'IMAGE', 'GEMINI', 'NORTH', 'PREPARED', 'SIDEREAL', 'NIRI'}
    Settable parameters on 'display':
    ========================================
     Name			Current setting

    extname              'SCI'                EXTNAME to display
    frame                1                    Starting frame for display
        Valid Range = [1,inf)
    ignore               False                Turn off display?
    overlay              None                 Overlays for display
    threshold            'auto'               Threshold level for indicating saturation
    tile                 True                 Tile multiple extensions into single display frame?
    zscale               True                 Use zscale algorithm?


The ``-p`` flag is used to modify primitive input parameters.  The syntax is::

    reduce filename.fits -p primitive:parameter=value

If the "primitive" part is not specified, *all* the primitives with "parameter"
will be assigned a new value.  Be careful.

::

    reduce -r display N20160102S0271_stack.fits -p display:frame=2

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

If you wanted to change the interval between the displays::

    cat stdstar.lis  # to get the name of one of the files for showpars
    showpars ../playdata/N20160102S0295.fits inspect

    Dataset tagged as {'UNPREPARED', 'RAW', 'SIDEREAL', 'NIRI', 'GEMINI', 'NORTH', 'IMAGE'}
    Settable parameters on 'inspect':
    ========================================
     Name			Current setting

    extname              'SCI'                EXTNAME to display
    frame                1                    Starting frame for display
        Valid Range = [1,inf)
    ignore               False                Turn off display?
    overlay              None                 Overlays for display
    threshold            'auto'               Threshold level for indicating saturation
    tile                 True                 Tile multiple extensions into single display frame?
    zscale               True                 Use zscale algorithm?
    pause                2                    Pause between the display, in seconds
        Valid Range = [0,inf)

::

    reduce -r inspect @stdstar.lis -p inspect:pause=1

    reduce -r inspect @stdstar.lis -p pause=1

