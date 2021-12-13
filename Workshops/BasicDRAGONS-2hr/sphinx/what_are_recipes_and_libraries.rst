.. what_are_recipes_and_libraries.rst

.. include:: DRAGONSlinks.txt

.. _recipes:

*************************************
What are Recipes and Recipe Libraries
*************************************
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

Let's look at examples and tools.

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
argument is the **primitive set** that matches the input.  We will talk about
that in the next chapter.

Exploring recipes
-----------------
|showrecipes| can be used to see which recipe DRAGONS with run by default
given the input data.  The tool's basic usage signature is::

    showrecipes fitsfile_name.fits

To see more options, try ``showrecipes -h``.

.. _ex_recipes1:

.. admonition:: Exercise - Recipes 1

   The file ``N20160102S0373.fits``, in the ``playdata`` directory, is a raw
   "lamp-on" flat.

   * Get the default recipe for a NIRI flat displayed on the terminal
   * Find the name of the default recipe.

   [:ref:`Solution <solution_recipes1>`]

.. showrecipes ../playdata/N20160102S0373.fits

.. p.prepare()
   p.addDQ()
   p.addVAR(read_noise=True)
   p.nonlinearityCorrect()
   p.ADUToElectrons()
   p.addVAR(poisson_noise=True)
   p.makeLampFlat()
   p.normalizeFlat()
   p.thresholdFlatfield()
   p.storeProcessedFlat()

.. makeProcessedFlat


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
"IMAGE", "FLAT".  The tags of the first file in the list of inputs are used
by the matching algorithm.  Each recipe library is assigned a set of tags that
defines which type of data this library is for.

.. _ex_recipes2:

.. admonition:: Exercise - Recipes 2

   The file ``N20160102S0423.fits`` in the ``playdata`` directory is a raw dark.

   * Find the location of the recipe library on your disk.
   * What are the tags assigned to the input file?
   * What are the tags assigned to that recipe library?

   [:ref:`Solution <solution_recipes2>`]


.. showrecipes ../playdata/N20160102S0423.fits

.. Recipe location:  ...../geminidr/niri/recipes/sq/recipes_DARK.py
.. Input tags: ['DARK', 'RAW', 'AT_ZENITH', 'NORTH', 'AZEL_TARGET', 'CAL',
                'UNPREPARED', 'GEMINI', 'NIRI', 'NON_SIDEREAL']
.. Recipe tags: set(['DARK', 'NIRI', 'CAL'])

As mentioned above, a recipe library can have more than one recipe.  To see
the list of recipes, |showrecipes| has the option ``--all``.  Then, to see the
sequence of primitives for one of those recipes, there's the option ``-r``.

DRAGONS has the concept of "reduction mode".  There are three modes:
the science quality mode, "sq", the quicklook mode, "ql", and the quality
assessment mode, "qa".  You can safely ignore the "qa" mode, it is used
exclusively at the observatory, at night, to help with the assessment of the
sky conditions and the resulting quality of the data.  The mode can be
specified in |showrecipes| with the ``-m`` flag.  The default is "sq".

To see all the flags and option, ``showrecipes -h``.

.. _ex_recipes3:

.. admonition:: Exercise - Recipes 3

   For the file ``N20160102S0270.fits`` in the ``playdata`` directory:

   * List all matching recipes.  Note the "sq", and the "qa" recipes.
   * Show the ``makeSkyFlat`` recipe.  ("sq" is the default.)
   * Show the ``reduce`` recipe for "qa" mode.

   [:ref:`Solution <solution_recipes3>`]


.. showrecipes ../playdata/N20160102S0270.fits --all

.. geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
   geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.sq.recipes_IMAGE::reduce
   geminidr.niri.recipes.qa.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.qa.recipes_IMAGE::reduce

.. showrecipes ../playdata/N20160102S0270.fits -r makeSkyFlat

.. showrecipes ../playdata/N20160102S0270.fits -r reduce -m qa

