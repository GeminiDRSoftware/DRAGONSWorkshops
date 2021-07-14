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


