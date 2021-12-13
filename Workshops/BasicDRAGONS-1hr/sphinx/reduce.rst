.. reduce.rst

.. include:: DRAGONSlinks.txt

.. _basic1_reduce:

****************
Using ``reduce``
****************

The ``reduce`` command (and the ``Reduce`` class if you are using the
programming interface) is what drives DRAGONS.  The command has a lot of
options.  We have used a few already.  Let's review those and show a few more.

First, to see the list of options available, use the ``-h`` flag::

    reduce -h


Setting the suffix of the final outputs
=======================================
It is not possible to set the name of the final output.  This seems odd, but
here is why.  There might be more than one output, there can be dozens, it
depends on the recipe.  It is not reasonable to ask the user for a dozen
different output strings on the command line, it also interferes with the
automation.

We are still exploring solutions to this problem.

In the meantime, the user can set the suffix of the outputs.  Each primitive
has a meaningful suffix attached to it, for example ``_sourcesDetected`` for
the primitive ``detectSources``.  The output of a ``reduce`` call will get
the suffix assigned to the last primitive of the recipe.

If you wish to override that, you can set the ``--suffix`` option.

::

    reduce @targets --suffix=_mytest

Customizing parameters
======================
We have seen earlier how to check the available input parameters and their
default settings for a given frame and primitive.  The command |showpars| does
that.  For example::

    showpars ../playdata/N20160102S0363.fits normalizeFlat

The syntax to change the value of an input parameter is as follow::

    reduce file.fits -p primitive:parameter=value

.. _basic1_ex_reduce1:

.. admonition:: Exercise - "reduce" 1

    White down the ``reduce`` command to reduce the flats from the demo
    (``flats.lis``) but this time set the scaling for ``normalizeFlat`` flat
    to ``mean``.  To avoid overwriting our "real" processed
    flat, let's set the suffix to ``_exercise1``.

    Hint #1: ``cat flats.lis`` to get the name of a flat to use with ``showpars``.

    Hint #2: See the :ref:`command used in the demo <basic1_demo_flat>`

    [:ref:`Solution <basic1_solution_reduce1>`]

.. reduce @flats.lis -p normalizeFlat:scale=mean --suffix _exercise2


Running non-default recipes
===========================
A recipe library often contains several recipes.  One recipe in the library
is set as the default library.  If one wants to run a different library,
its name can be specified on the command line using the ``-r`` flag.

As we have seen before to see the list of all available recipes, we can use the
|showrecipes| command with the ``--all`` flag.

::

    showrecipes ../playdata/N20160102S0270.fits --all

    Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/N20160102S0270.fits
    Input tags: {'GEMINI', 'NORTH', 'NIRI', 'UNPREPARED', 'SIDEREAL', 'IMAGE', 'RAW'}
    Recipes available for the input file:
       geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
       geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
       geminidr.niri.recipes.sq.recipes_IMAGE::reduce
       geminidr.niri.recipes.qa.recipes_IMAGE::makeSkyFlat
       geminidr.niri.recipes.qa.recipes_IMAGE::reduce
       geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
       geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
       geminidr.niri.recipes.sq.recipes_IMAGE::reduce

The strings ``sq`` and ``qa`` refer to the reduction mode.  The ``qa`` mode,
Quality Assessment, is used internally at Gemini.   General users will be
using the ``sq``, Science Quality, recipes.

.. warning:: The last three "sq" recipes
    in the list above are really the "ql", Quicklook, recipes.  This a newly
    discovered bug (circa Dec 2021).  The NIRI quicklook recipes are identical to
    the science recipes and are just "Python-imported" from the science module,
    and that import trips the current implementation of ``showrecipes``.

The reduction mode is ``sq`` by default.  To change that, one uses the ``--qa``
or soon ``--ql`` flags with ``reduce``.  We will be using the science quality
recipes here so we do not need those flags in this version of workshop.

The syntax to select the specific recipe we want to use is::

    reduce file1.fits file2.fits -r recipename   (optional --ql/--qa)

The ``-r`` option can also be used to primitives as we have seen elsewhere.
Just type the name of the primitive instead of the recipe::

    reduce file1.fits file2.fits -r primitivename

Another use of the ``-r`` option is to run personal recipes rather than the
ones distributed with DRAGONS.  We will show how that is done in the next
chapter.

.. _basic1_ex_reduce2:

.. admonition:: Exercise - "reduce" 2

    Write the command to use the ``makeSkyFlat`` recipe on the science frames.
    Set the suffix to ``_skyflat``.

    Note that because
    the target fills the field-of-view of the science frames, the sky flat
    in this particular case will not be usable, but it's okay, we are just
    exploring the ``reduce`` command-line here.

    [:ref:`Solution <basic1_solution_reduce2>`]

.. reduce @target.lis -r makeSkyFlat --suffix _skyflat


Overriding the calibration selection
====================================
If you wish to force DRAGONS to use a specific processed calibration, overriding
the automatic selection, you can use the ``--user_cal`` flag.  Here is a
usage example.

::

    reduce file1.fits file2.fits --user_cal processed_<cal>:my_cal.fits

Allowed values for "<cal>": dark, bias, flat, arc, standard.  Eg::

    reduce @mylist.lis --user_cal processed_arc:my_special_arc.fits

.. _basic1_ex_reduce3:

.. admonition:: Exercise - "reduce" 3

    In the demo, we reduced the flux standard as follow:

    ::

        reduce @stdstar.lis -p darkCorrect:do_cal=skip

    Modify this command to allow the dark correction to use the processed
    dark we used for the science frame, ``N20160102S0423_dark.fits``.

    [:ref:`Solution <basic1_solution_reduce3>`]

.. reduce reduce @stdstar.lis -p --user_cal processed_dark:N20160102S0423_dark.fits
