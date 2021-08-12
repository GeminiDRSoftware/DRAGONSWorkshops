.. tools.rst

.. include:: DRAGONSlinks.txt

.. |suptools| raw:: html

    <a href="https://dragons.readthedocs.io/projects/recipe-system-users-manual/en/stable/supptools.html" target="_blank">Supplemental Tools</a>

.. |rsusermanual| raw:: html

    <a href="https://dragons.readthedocs.io/projects/recipe-system-users-manual/en/stable/index.html" target="_blank">Recipe System User Manual</a>



.. _tools:




*****
Tools
*****

DRAGONS offers some tools to help with the bookkeeping.  We have used
``dataselect`` already to create lists, we used ``showpars`` and ``showrecipes``
to check the primitive parameter settings and the recipes.

Here we will explore ``dataselect`` a bit more, and introduce ``showd`` and
``typewalk``.

Additional information about the tools can be found in the |suptools| chapter
of the |rsusermanual|.


typewalk
========
The oddly-named ``typewalk`` tool allows the user the list the astrodata tags
(formerly "types") of all the files in a directory, and can recurse ("walk")
through subdirectories.  It can also be used to select data on tags, but we
recommend using the much more flexible ``dataselect`` for that.

To see the type of data files that we have in ``playdata`` and see which
tags are available for selection::

    typewalk --dir ../playdata

    directory:  /Users/klabrie/data/tutorials/niriimg_tutorial/playdata
     N20160102S0270.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0271.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0272.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0273.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0274.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0275.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0276.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0277.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0278.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0279.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0295.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0296.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0297.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0298.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0299.fits ............... (GEMINI) (IMAGE) (NIRI) (NORTH) (RAW) (SIDEREAL) (UNPREPARED)
     N20160102S0363.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0364.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0365.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0366.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0367.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0368.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0369.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0370.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0371.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0372.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_OFF) (GEMINI) (IMAGE) (LAMPOFF) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0373.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0374.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0375.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0376.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0377.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0378.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0379.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0380.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0381.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0382.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (FLAT) (GCALFLAT) (GCAL_IR_ON) (GEMINI) (IMAGE) (LAMPON) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0423.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0424.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0425.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0426.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0427.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0428.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0429.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0430.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0431.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160102S0432.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0463.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0464.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0465.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0466.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0467.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0468.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0469.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0470.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0471.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)
     N20160103S0472.fits ............... (AT_ZENITH) (AZEL_TARGET) (CAL) (DARK) (GEMINI) (NIRI) (NON_SIDEREAL) (NORTH) (RAW) (UNPREPARED)


From that output we see the darks with their ``DARK`` tag, the flats with the
``FLAT`` tags, some with ``LAMPON``, some with ``LAMPOFF``.  The science frames
do not have the ``CAL`` (for calibration) tag.

showd
=====

``typewalk`` shows tags, ``showd`` shows the values of astrodata |descriptors|.
The list available descriptor is included in an appendix of the |astrodatauser|.

The syntax is::

    showd -d descriptor_name filenames
    showd -d descriptor1,descriptor2,descriptorN filenames

The default is to print on the terminal.  A comma-separate list can be
produced with the ``--csv`` flag.

.. _ex_tools1:

.. admonition:: Exercise - Tools 1

    Get the exposure time, filter name, and UT date of all the
    observations in ``playdata``.  Use the |descriptors| list from the
    |astrodatauser|.

    [:ref:`Solution <solution_tools1>`]

.. showd -d exposure_time,filter_name,ut_date ../playdata/*.fits


dataselect
==========

``dataselect`` is a powerful tool to create list of exactly the files you want
based on tags and descriptors values.

You can include tags, exclude tags, and use Python comparison operators on the
descriptors.

Here are a few examples.

Select only darks::

    dataselect ../playdata/*.fits --tags DARK

Select only **non**-darks::

    dataselect ../playdata/*.fits --xtags DARK

Select lamp-on flats::

    dataselect ../playdata/*.fits --tags FLAT,LAMPON

Select any frames with exposure time of 10 seconds::

    dataselect ../playdata/*.fits --expr="exposure_time==10"

Select 20-second darks::

    dataselect ../playdata/*.fits --tags DARK --expr="exposure_time==20"

Select any frames observed on or after 2016-01-03::

    dataselect ../playdata/*.fits --expr="ut_date>='2016-01-03'"

We can combine ``dataselect`` and ``showd``.  Here we are going to get the
observation type, as defined in the Gemini Observing Tool (OT), for those
frames observed on or after 2016-01-03::

    dataselect ../playdata/*.fits --expr="ut_date>='2016-01-03'" | showd -d observation_type

The expression in ``--expr`` is a Python expression using the Python syntax.
Note the double ``==`` for equality in the examples above.  Using a single ``=``
is a common mistake.

Also, be careful with the quotes.  The external quotes must be different from
the internal quotes used around strings.

Finally, the expression can use the ``and`` and ``or`` logical operators.

.. _ex_tools2:

.. admonition:: Exercise - Tools 2

    Knowing that the science frames and the flux standard have an observation
    type of 'OBJECT', and that the science frames observation class is
    "science" while the flux standards are "partnerCal", create a list of
    science frames and a list of flux standards **without** using the object
    names.   Send the output to ``showd`` to print the object name.

    Descriptors:  ``observation_type``, ``observation_class``, ``object``.

    [:ref:`Solution <solution_tools2>`]

.. dataselect ../playdata/*.fits --expr='observation_class=="science" and observation_type=="OBJECT"' | showd -d object
.. dataselect ../playdata/*.fits --expr='observation_class=="partnerCal" and observation_type=="OBJECT"' | showd -d object



