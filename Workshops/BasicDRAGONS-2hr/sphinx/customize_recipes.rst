.. customize_recipes.rst

.. include:: DRAGONSlinks.txt

.. _customize_recipes:

*****************
Customize Recipes
*****************

Sometimes the DRAGONS recipes are not quite what is needed.  It is possible
to customize a recipe and have DRAGONS run it.  The easiest way to customize
a recipe is to find the most appropriate one, copy it over and edit it.

Here we will show you how to write intermediate results to disk for inspection.

The first step is to find a recipe and copy it to the local directory.  Let's
find the default NIRI science imaging recipe.

::

    showrecipes ../playdata/N20160102S0270.fits

The recipe name and the recipe library location returned will look like this:

::

    ...
    Input recipe: reduce
    ...
    Recipe location: /Users/klabrie/condaenvs/public3.7_3.0.1_20211206/lib/python3.7/site-packages/geminidr/niri/recipes/sq/recipes_IMAGE.py
    ...

.. note:: The file might end with ``.pyc`` instead of ``.py``.  This is the
    compiled file, the code file is in the same location and named ``.py``.  That
    is the file we will copy over.  It is not clear at this time why the ``.pyc``
    is sometimes returned instead of the ``.py`` filename.

::

    cp /Users/klabrie/condaenvs/public3.7_3.0.1_20211206/lib/python3.7/site-packages/geminidr/niri/recipes/sq/recipes_IMAGE.py .

You should now have a file named ``recipes_IMAGE.py`` in your current directory.
Let's rename it for clarity::

    mv recipes_IMAGE.py myNIRIrecipes.py

Use your favorite editor to open ``myNIRIrecipes.py`` to edit the recipe named
``reduce``.  We will save some intermediate results:  the flat corrected files
and the sky corrected frames before alignment and stacking.  To do that we
need to add ``writeOutputs`` after ``flatCorrect`` and ``skyCorrect``.

.. code-block:: python
    :emphasize-lines: 9,18

    p.prepare()
    p.addDQ()
    p.removeFirstFrame()
    p.ADUToElectrons()
    p.addVAR(read_noise=True, poisson_noise=True)
    p.nonlinearityCorrect()
    p.darkCorrect()
    p.flatCorrect()
    p.writeOutputs()
    p.separateSky()
    p.associateSky(stream='sky')
    p.skyCorrect(instream='sky', mask_objects=False, outstream='skysub')
    p.detectSources(stream='skysub')
    p.transferAttribute(stream='sky', source='skysub', attribute='OBJMASK')
    p.clearStream(stream='skysub')
    p.associateSky()
    p.skyCorrect(mask_objects=True)
    p.writeOutputs()
    p.detectSources()
    p.adjustWCSToReference()
    p.resampleToCommonFrame()
    p.stackFrames()
    p.writeOutputs()
    return

Note that some of the primitives in the recipe have parameters set in the
recipe itself.  Parameter values set in a recipe will always take precedence.
Those cannot be changed by the user from the command line.

Now that we have an edited recipe that will write the flat corrected and the
pre-alignment sky corrected frames, how do we run it?   With ``-r`` of course!
But the syntax is just a touch different.  We need to specific both the
name of our recipe library (``myNIRIrecipes``) and the name of the recipe
(``reduce``).

::

    reduce @target.lis -r myNIRIrecipes.reduce

The new intermediate outputs have the suffixes ``_flatCorrected`` and
``skySubtracted``.

.. _ex_customrecipe1:

.. admonition:: Exercise - Custom Recipe 1

    Edit the master flat recipe to write the pre-normalized flat to disk, and
    to display the master flat at the end of the recipe.

    Hint: ``cat flats.lis`` to get the name of a flat to use with ``showrecipes``
    to get the location and name of the NIRI flat recipe library.

    [:ref:`Solution <solution_customrecipe1>`]

