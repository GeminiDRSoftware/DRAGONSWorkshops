.. explore_data.rst

.. .. include:: DRAGONSlinks.txt

.. _explore_data:


************
Explore data
************

Once we have reduced our data, we probably want to look at.  In fact,
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

Inspect
=======

It is recommended to inspect the raw data prior to reducing the data.  A quick
visual inspection can catch obviously bad data, or flag some frames for
more detailed inspection.  Having to run display on each frame individually
would be painful.  The primitive ``inspect`` can be used here.  It
accepts a list of file and will display each frame in sequence, adding a short
pause between the frames.  The length of the pause can be controlled with
the ``pause`` parameter.

::

    reduce -r inspect @stdstar.lis

.. _ex_explore1:

.. admonition:: Exercise - Explore 1

   #. Display ``N20160102S0271_stack.fits`` to buffer 2 and turn off the
      saturation marking.
   #. Reduce the time between frames to 1 second in ``inspect``.

   Hint: ``reduce file.fits -r primitive -p primitive:parameter=value``

   [:ref:`Solution <solution_explore1>`]


.. reduce -r display N20160102S0271_stack.fits -p frame=2 threshold=None

.. Note above that `display:` is not used.  When the primitive is not
   specified, it applies to all parameters with that name from any primitives.
   We get away with it here because only the `display` primitive is being run.

.. reduce -r inspect @stdstar.lis -p pause=1


Accessing table content
=======================
Some output files will have table information.  Once written to disk as a FITS
file, the tables are accessible as any FITS files.  A common table is a list
of sources detected by the primitive ``detectSources``.

Let say that we want to get position of the sources in the flux standard stack
we produced in the previous chapter.  First, we run ``detectSources`` since
it was not run after the stack was created.

::

    reduce -r detectSources N20160102S0296_stack.fits

This will add a table with object coordinates, magnitudes, etc.  The output file
is ``N20160102S0296_sourcesDetected.fits``.  Use your preferred tool to access
the FITS table named ``OBJCAT`` from that output.  Or you can use DRAGONS'
``astrodata`` to access the table as an Astropy ``Table``.

.. code-block:: python
    :linenos:

    import astrodata
    import gemini_instruments

    ad = astrodata.open('N20160102S0296_sourcesDetected.fits')
    ad.info()

::

    Filename: N20160102S0296_sourcesDetected.fits
    Tags: GEMINI IMAGE NIRI NORTH PREPARED SIDEREAL

    Pixels Extensions
    Index  Content                  Type              Dimensions     Format
    [ 0]   science                  NDAstroData       (1195, 1195)   float32
              .variance             ndarray           (1195, 1195)   float32
              .mask                 ndarray           (1195, 1195)   uint16
              .OBJCAT               Table             (29, 43)       n/a
              .OBJMASK              ndarray           (1195, 1195)   uint8


.. code-block:: python
    :linenos:
    :lineno-start: 6

    ad[0].OBJCAT

::

    <Table length=29>
    NUMBER  X_IMAGE   Y_IMAGE        ERRX2_IMAGE       ... REF_MAG REF_MAG_ERR PROFILE_FWHM PROFILE_EE50
    int32   float32   float32          float64         ... float32   float32     float32      float32
    ------ --------- ---------- ---------------------- ... ------- ----------- ------------ ------------
         1 638.74554   7.358276  9.678123990684229e-06 ...  -999.0      -999.0       -999.0       -999.0
         2   52.5378  15.138286  0.0007821264390706773 ...  -999.0      -999.0          0.0    3.5255525
         3 500.59454  1146.9167   0.012262252608200411 ...  -999.0      -999.0     4.513517    6.1514697
         4 879.47473  1098.4614  2.762548642098426e-05 ...  -999.0      -999.0     4.222008    5.0135765
         5  99.23588  1012.4829  7.044837003541005e-06 ...  -999.0      -999.0    3.5682483    4.5748672
         6   67.8556 1023.36334  0.0036609541234138973 ...  -999.0      -999.0    3.7424104    4.9484925
         7   654.096  918.19916 2.5297123639012174e-05 ...  -999.0      -999.0    4.6524267     4.829588
         8 855.02795   830.9594  6.732778544641382e-06 ...  -999.0      -999.0     6.675581    6.2657185
         9 1026.4033    835.157     0.0433908185530153 ...  -999.0      -999.0     4.068429    12.851359
        10  380.1143  726.31464 0.00041709156417067195 ...  -999.0      -999.0     4.370194    4.4451947
        11 994.73755    721.932   0.012686314238205688 ...  -999.0      -999.0    4.6524267      9.30962
       ...       ...        ...                    ... ...     ...         ...          ...          ...
        18  854.6116  352.42896 1.5660364975379077e-05 ...  -999.0      -999.0     4.222008     4.193287
        19 522.56934  317.01556  0.0012430236331253412 ...  -999.0      -999.0    3.5682483     3.738548
        20 936.59576  242.70079  7.264929326425364e-06 ...  -999.0      -999.0     4.068429    4.1155357
        21  811.8014  225.92892   8.23835737153326e-05 ...  -999.0      -999.0    3.9088202    4.1611533
        22 478.92426  217.57622    0.02797088088581828 ...  -999.0      -999.0     8.812923       -999.0
        23  479.3753   206.3997   0.022581652547939684 ...  -999.0      -999.0    12.257336    12.230416
        24 478.64758  194.86026   0.009814622938623417 ...  -999.0      -999.0    2.7639532    15.229653
        25 521.57794  209.82019   0.029336651973282495 ...  -999.0      -999.0    3.1915383    16.864668
        26  479.6435  152.71979 3.3380840555892644e-06 ...  -999.0      -999.0    3.9088202    3.7304442
        27 955.26416   150.0823  7.081766055952015e-05 ...  -999.0      -999.0    3.7424104     4.108046
        28 561.57697  151.73418   0.021476893612064937 ...  -999.0      -999.0     6.675581     9.169356
        29 129.58281  107.06496  0.0009807660866281128 ...  -999.0      -999.0    3.3851376    3.6767333

See the ``astropy`` documentation for information about how to operate on
``Table`` objects: https://docs.astropy.org/en/stable/table/.

For more on ``astrodata``, see the |astrodatauser|.


