.. solutions.rst

.. include:: DRAGONSlinks.txt

.. _solutions:

**********************
Solutions to exercises
**********************

Solutions to Recipe and recipe libraries exercises
==================================================

.. _solution_recipes1:

Solution to :ref:`Exercise - Recipes 1 <ex_recipes1>`
-----------------------------------------------------
The |showrecipes| command is used on the file.  Highlighted below is the
name of the default recipe, ``makeProcessedFlat``, and the primitives that
will be run.

.. code-block::
   :emphasize-lines: 3,7,11,12,13,14,15,16,17,18,19,20,21

    showrecipes ../playdata/N20160102S0373.fits

    Recipe not provided, default recipe (makeProcessedFlat) will be used.
    Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/N20160102S0373.fits
    Input tags: ['FLAT', 'NORTH', 'AZEL_TARGET', 'GEMINI', 'IMAGE', 'LAMPON', 'GCALFLAT', 'RAW', 'AT_ZENITH', 'NON_SIDEREAL', 'CAL', 'UNPREPARED', 'NIRI', 'GCAL_IR_ON']
    Input mode: sq
    Input recipe: makeProcessedFlat
    Matched recipe: geminidr.niri.recipes.sq.recipes_FLAT_IMAGE::makeProcessedFlat
    Recipe location: /Users/klabrie/condaenvs/gemini2_2.1.x_20200925/lib/python2.7/site-packages/dragons-2.1.1-py2.7-macosx-10.7-x86_64.egg/geminidr/niri/recipes/sq/recipes_FLAT_IMAGE.pyc
    Recipe tags: set(['FLAT', 'IMAGE', 'NIRI', 'CAL'])
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

.. _solution_recipes2:

Solution to :ref:`Exercise - Recipes 2 <ex_recipes2>`
-----------------------------------------------------
Again |showrecipes| is used.  The recipe location, the input file tags,
and the recipe tags are highlighted.

.. code-block::
   :emphasize-lines: 5, 7, 10

   showrecipes ../playdata/N20160102S0423.fits

   Recipe not provided, default recipe (makeProcessedDark) will be used.
   Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/N20160102S0423.fits
   Input tags: ['DARK', 'RAW', 'AT_ZENITH', 'NORTH', 'AZEL_TARGET', 'CAL', 'UNPREPARED', 'GEMINI', 'NIRI', 'NON_SIDEREAL']
   Input mode: sq
   Input recipe: makeProcessedDark
   Matched recipe: geminidr.niri.recipes.sq.recipes_DARK::makeProcessedDark
   Recipe location: /Users/klabrie/condaenvs/gemini2_2.1.x_20200925/lib/python2.7/site-packages/dragons-2.1.1-py2.7-macosx-10.7-x86_64.egg/geminidr/niri/recipes/sq/recipes_DARK.pyc
   Recipe tags: set(['DARK', 'NIRI', 'CAL'])
   Primitives used:
      p.prepare()
      p.addDQ(add_illum_mask=False)
      p.addVAR(read_noise=True)
      p.nonlinearityCorrect()
      p.ADUToElectrons()
      p.addVAR(poisson_noise=True)
      p.stackDarks()
      p.storeProcessedDark()


.. _solution_recipes3:

Solution to :ref:`Exercise - Recipes 3 <ex_recipes3>`
-----------------------------------------------------
The option ``--all`` is used to show all the recipes.  Note in the module
path the ``sq`` and the ``qa``.  The recipe names can be the same but their
content will differ depending on the reduction mode selected.  Default is
always ``sq``, science quality.

.. code-block::

   showrecipes ../playdata/N20160102S0270.fits --all

   geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
   geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.sq.recipes_IMAGE::reduce
   geminidr.niri.recipes.qa.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.qa.recipes_IMAGE::reduce



To see the content of a specific recipe, name it with the ``-r`` flag.

.. code-block::

   showrecipes ../playdata/N20160102S0270.fits -r makeSkyFlat

Finally, to print the content of the quality assessment, "qa", recipe
named ``reduce`` (also the default in that ``qa`` library), use the
``-m`` flag:

.. code-block::

   showrecipes ../playdata/N20160102S0270.fits -r reduce -m qa


Solutions to Primitives exercises
==================================================

.. _solution_primitives1:

Solution to :ref:`Exercise - Primitives 1 <ex_primitives1>`
-----------------------------------------------------------

The first step is get yourself familiarized with the primitive names.  This
can be done by looking at the recipe.

.. code-block::

   showrecipes ../playdata/N20160102S0270.fits

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

**Question 1:**

This question is about sky correction.  The primitive ``skyCorrect`` is
a good bet.

.. code-block::

   showpars ../playdata/N20160102S0270.fits skyCorrect

   ...
   operation            'median'             Averaging operation
   Allowed values:
       wtmean	variance-weighted mean
       mean	arithmetic mean
       median	median
       lmedian	low-median
   ...

The default for combining the sky frames is ``median``.

**Question 2:**

The second question asks which paramter controls whether or not the dark
correction will be run.  Let's look at ``darkCorrect``.

.. code-block::
   :emphasize-lines: 10

   showpars ../playdata/N20160102S0270.fits darkCorrect

   Dataset tagged as set(['RAW', 'GEMINI', 'NORTH', 'SIDEREAL', 'UNPREPARED', 'IMAGE', 'NIRI'])
   Settable parameters on 'darkCorrect':
   ========================================
    Name			Current setting

   suffix               '_darkCorrected'     Filename suffix
   dark                 None                 Dark frame
   do_dark              True                 Perform dark subtraction?

The parameter ``do_dark`` controls the dark correction.

**Question 3:**

The last question is about the stacking of the reduced frames.  At the end
of the recipe there's the primitive ``stackFrames``.

.. code-block::

   showpars ../playdata/N20160102S0270.fits stackFrames

   ...
   reject_method        'varclip'            Pixel rejection method
   Allowed values:
       minmax	reject highest and lowest pixels
       none	no rejection
       varclip	reject pixels based on variance array
       sigclip	reject pixels based on scatter
   ...

The ``reject_method`` is the answer.  It can be set to ``minmax``, to ``none``,
to ``varclip`` (currently the default), or to ``sigclip``.

