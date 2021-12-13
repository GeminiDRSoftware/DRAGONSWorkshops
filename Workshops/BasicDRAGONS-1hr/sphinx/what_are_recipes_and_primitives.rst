.. what_are_recipes_and_primitives.rst

.. include:: DRAGONSlinks.txt

.. _basic1_recipes_and_primitives:

**************************************************
What are Recipes, Recipe Libraries, and Primitives
**************************************************
A DRAGONS **recipe** is a set of instructions, called **primitives**, that
processes data in a certain way.  Depending on the recipe, the input data
can be in any state of processing.  Most often though, people will want to
process Gemini raw data into a master calibration or a processed calibrated
output.

A **recipe library** is a collection of recipes.  The recipes in a library have
one thing in common:  *they all apply to the same type of data*.  When DRAGONS
search for a matching recipe for some input data, it searches for a recipe
library.  In each library, one recipe is set as the default recipe.  To use
the others, the user needs to specify the name of the non-default recipes.

A **primitive** is the function that does something to the data.  The name
of the primitive normally gives you a good idea of what its purpose is.

Recipes
=======

This is what a recipe looks like:

.. code-block:: python

    def reduce(p):
       p.prepare()
       p.addDQ()
       p.removeFirstFrame()
       p.ADUToElectrons()
       p.addVAR(read_noise=True, poisson_noise=True)
       p.nonlinearityCorrect()
       p.darkCorrect()
       p.flatCorrect()
       p.separateSky()
       p.associateSky(stream='sky')
       p.skyCorrect(instream='sky', mask_objects=False, outstream='skysub')
       p.detectSources(stream='skysub')
       p.transferAttribute(stream='sky', source='skysub', attribute='OBJMASK')
       p.clearStream(stream='skysub')
       p.associateSky()
       p.skyCorrect(mask_objects=True)
       p.detectSources()
       p.adjustWCSToReference()
       p.resampleToCommonFrame()
       p.stackFrames()
       p.writeOutputs()


A recipe is a Python function that calls "primitives" sequentially.  The user
does not need to know Python to understand more or less what is being done
to the data when the recipe is run.  From the recipe above, we can read that
the data will be corrected for dark current, for flat field effects, the
sky background will be subtracted and the frames will be stacked.  The `p`
argument is the **primitive set** that matches the input.


Recipe libraries
================

Recipes are stored in recipe libraries, or in Python language, a module.  A
recipe library can have one or more recipes in them.  One recipe is identified
as the default recipe.

When DRAGONS searches for a recipe, it is actually searching for a matching
recipe library.  Once found, it will run the default recipe, unless instructed
otherwise by the user.

The recipe libraries are associated with the input files by matching
``astrodata`` tags (|astrodatauser|).  The tags are qualifiers like "NIRI",
"IMAGE", "FLAT".  The tags of the
first file in the list of inputs are used.  Each recipe library is assigned a
set of tags that defines which type of data this library is for.


Primitives and primitive sets
=============================
A primitive is a data reduction step involving a transformation of the data or
providing a service. By convention, the primitives are named to convey the
scientific meaning of the transformation. For example ``biasCorrect`` will
remove the bias signal from the input data.

A primitive is always a member of a primitive set. It is the primitive set
that gets matched to the data by the Recipe System, not the individual
primitives.

Technically, a primitive is a method of a primitive class. A primitive class
gets associated with the input dataset by matching the ``astrodata`` tags. Once
associated, all the primitives in that class, locally defined or inherited,
are available to reduce that dataset. We refer to that collection of
primitives as a “primitive set”.


Primitive input parameters
==========================
Also attached to a primitive set are the input parameters for each primitives
and the defaults appropriate for that primitive set, that is for that type
of data.

For the same generic primitive, the default values for input parameters for
NIRI data can be different from the defaults applicable to GMOS data.  Even
the set of available input parameters can be different, though we try to
keep things as uniform as possible.


Exploring recipes and primitives
================================

showrecipes
-----------

|showrecipes| allows the user to see which recipe library gets picked up by the
system and which recipe is run by default, which others are available.

The syntax is::

    showrecipes fitsfile_name.fits

Let's look at a couple examples.  From the ``niriimg_tutorial/playdata``
directory::

    Recipe not provided, default recipe (makeProcessedFlat) will be used.
    Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/N20160102S0373.fits
    Input tags: ['GCALFLAT', 'NORTH', 'AT_ZENITH', 'NON_SIDEREAL', 'AZEL_TARGET', 'RAW', 'IMAGE', 'GCAL_IR_ON', 'NIRI', 'GEMINI', 'UNPREPARED', 'LAMPON', 'CAL', 'FLAT']
    Input mode: sq
    Input recipe: makeProcessedFlat
    Matched recipe: geminidr.niri.recipes.sq.recipes_FLAT_IMAGE::makeProcessedFlat
    Recipe location: /Users/klabrie/condaenvs/public3.7_3.0.1_20211206/lib/python3.7/site-packages/geminidr/niri/recipes/sq/recipes_FLAT_IMAGE.py
    Recipe tags: {'FLAT', 'IMAGE', 'CAL', 'NIRI'}
    Primitives used:
       p.prepare()
       p.addDQ()
       p.addVAR(read_noise=True)
       p.nonlinearityCorrect()
       p.ADUToElectrons()
       p.addVAR(poisson_noise=True)
       p.makeLampFlat()
       p.normalizeFlat()
       p.thresholdFlatfield()
       p.storeProcessedFlat()

This contains: the name of the recipe, the location of the recipe library,
the :astrodata: tags of the input file, those assigned to the recipe library,
and the recipe itself.

A library can have more than one recipe, to see them all use the ``--all``
flag::

    showrecipes N20160102S0270.fits --all

    Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/N20160102S0270.fits
    Input tags: {'NIRI', 'NORTH', 'GEMINI', 'UNPREPARED', 'IMAGE', 'RAW', 'SIDEREAL'}
    Recipes available for the input file:
       geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
       geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
       geminidr.niri.recipes.sq.recipes_IMAGE::reduce
       geminidr.niri.recipes.qa.recipes_IMAGE::makeSkyFlat
       geminidr.niri.recipes.qa.recipes_IMAGE::reduce
       geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
       geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
       geminidr.niri.recipes.sq.recipes_IMAGE::reduce


The recipe library for science quality has three recipes: ``alignAndStack``,
``makeSkyFlat``, and ``reduce``.

.. note:: Regarding the "sq" and "qa" in the paths.
    DRAGONS has the concept of "reduction mode".  Right now, there are two modes:
    the science quality mode, "sq", and the quality assessment mode, "qa".  You
    can safely ignore the "qa" mode, it is used exclusively at the observatory, at
    night, to help with the assessment of the sky conditions and the resulting
    quality of the data.  Everything defaults to "sq".

    The last three "sq" recipes are really the "ql" recipes.  This a newly
    discovered bug (circa Dec 2021).  The NIRI quicklook recipes are identical to
    the science recipes and are just "Python imported" from the science module,
    and that import trips the current implementation of ``showrecipes``.

To see what a specific recipe looks like, not just the default recipe, use
the ``-r`` flag::

    showrecipes N20160102S0270.fits -r makeSkyFlat

    Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/N20160102S0270.fits
    Input tags: ['UNPREPARED', 'NORTH', 'NIRI', 'SIDEREAL', 'RAW', 'IMAGE', 'GEMINI']
    Input mode: sq
    Input recipe: makeSkyFlat
    Matched recipe: geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
    Recipe location: /Users/klabrie/condaenvs/gemini3_2.1.x_20200925/lib/python3.6/site-packages/dragons-2.1.1-py3.6-macosx-10.7-x86_64.egg/geminidr/niri/recipes/sq/recipes_IMAGE.py
    Recipe tags: {'IMAGE', 'NIRI'}
    Primitives used:
       p.prepare()
       p.addDQ()
       p.ADUToElectrons()
       p.addVAR(read_noise=True, poisson_noise=True)
       p.nonlinearityCorrect()
       p.darkCorrect()
       p.stackFrames(operation='median', scale=True, outstream='fastsky')
       p.normalizeFlat(stream='fastsky')
       p.thresholdFlatfield(stream='fastsky')
       p.flatCorrect(flat=p.streams['fastsky'][0], outstream='flattened')
       p.detectSources(stream='flattened')
       p.dilateObjectMask(dilation=10, stream='flattened')
       p.addObjectMaskToDQ(stream='flattened')
       p.writeOutputs(stream='flattened')
       p.transferAttribute(source='flattened', attribute='mask')
       p.stackFrames(operation='mean', scale=True, reject_method="minmax", nlow=0, nhigh=1)
       p.normalizeFlat()
       p.thresholdFlatfield()
       p.storeProcessedFlat(force=True)



showpars
--------

Primitive input parameters can be customized.  To see the available input
parameters, their default values, and range of allowed values, use |showpars|.
The syntax is::

    showpars filename.fits primitive_name

A dataset is required here because the parameters and their defaults do change
depending on the type of data.  Defaults between instruments can be different
because the data has different characteristics.  Also, the primitive might be
doing slightly different things (eg. optical vs near-IR).

Let's say that we want to reduce NIRI science data, an example of which is
file ``N20160102S0270.fits`` but we want to customize the sky correction,
the dark correction, and the rejection method during the final stack.  Here's
how one would proceed to check for primitive names and parameters names.

::

    showrecipes N20160102S0270.fits

    ...
       p.prepare()
       p.addDQ()
       p.removeFirstFrame()
       p.ADUToElectrons()
       p.addVAR(read_noise=True, poisson_noise=True)
       p.nonlinearityCorrect()
       p.darkCorrect()
       p.flatCorrect()
       p.separateSky()
       p.associateSky(stream='sky')
       p.skyCorrect(instream='sky', mask_objects=False, outstream='skysub')
       p.detectSources(stream='skysub')
       p.transferAttribute(stream='sky', source='skysub', attribute='OBJMASK')
       p.clearStream(stream='skysub')
       p.associateSky()
       p.skyCorrect(mask_objects=True)
       p.detectSources()
       p.adjustWCSToReference()
       p.resampleToCommonFrame()
       p.stackFrames()
       p.writeOutputs()


Here I spot the primtives:  ``skyCorrect``, ``darkCorrect``, and ``stackFrames``.


If I wanted to turn off the dark correction...

::

    showpars N20160102S0270.fits darkCorrect

    Dataset tagged as {'NORTH', 'IMAGE', 'NIRI', 'UNPREPARED', 'SIDEREAL', 'GEMINI', 'RAW'}
    Settable parameters on 'darkCorrect':
    ========================================
     Name			Current setting

    do_cal               'procmode'           Calibration requirement
    Allowed values:
        procmode	Use the default rules set by the processingmode.
        force	Require a calibration regardless of theprocessing mode.
        skip	Skip this correction, no calibration required.
        None	Field is optional

    suffix               '_darkCorrected'     Filename suffix
    dark                 None                 Dark frame

I would set ``do_cal`` to "skip".

If I wanted to change the ``operation`` done when combining sky frames to
a mean...

::

    showpars N20160102S0270.fits skyCorrect

    ...
    operation            'median'             Averaging operation
    Allowed values:
        mean	arithmetic mean
        wtmean	variance-weighted mean
        median	median
        lmedian	low-median
    ...

I would reset ``operation`` to ``mean``.

If I wanted to change the rejection method when doing the final stack...

::

    showpars N20160102S0270.fits stackFrames

    ...
    reject_method        'sigclip'            Pixel rejection method
    Allowed values:
        none	no rejection
        minmax	reject highest and lowest pixels
        sigclip	reject pixels based on scatter
        varclip	reject pixels based on variance array
    ...

I would pick one of the allowed value for ``reject_method``.

