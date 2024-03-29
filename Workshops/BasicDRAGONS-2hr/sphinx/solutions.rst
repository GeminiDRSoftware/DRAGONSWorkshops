.. solutions.rst

.. .. include:: DRAGONSlinks.txt

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

    showrecipes ../playdata/example1/N20160102S0373.fits

    Recipe not provided, default recipe (makeProcessedFlat) will be used.
    Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/example1/N20160102S0373.fits
    Input tags: ['GCALFLAT', 'NORTH', 'AT_ZENITH', 'NON_SIDEREAL', 'AZEL_TARGET', 'RAW', 'IMAGE', 'GCAL_IR_ON', 'NIRI', 'GEMINI', 'UNPREPARED', 'LAMPON', 'CAL', 'FLAT']
    Input mode: sq
    Input recipe: makeProcessedFlat
    Matched recipe: geminidr.niri.recipes.sq.recipes_FLAT_IMAGE::makeProcessedFlat
    Recipe location: /Users/klabrie/condaenvs/public3.10_3.1.0/lib/python3.10/site-packages/geminidr/niri/recipes/sq/recipes_FLAT_IMAGE.py
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

.. _solution_recipes2:

Solution to :ref:`Exercise - Recipes 2 <ex_recipes2>`
-----------------------------------------------------
Again |showrecipes| is used.  The recipe location, the input file tags,
and the recipe tags are highlighted.

.. code-block::
   :emphasize-lines: 5, 7, 10

   showrecipes ../playdata/example1/N20160102S0423.fits

   Recipe not provided, default recipe (makeProcessedDark) will be used.
   Input file: /Users/klabrie/data/tutorials/niriimg_tutorial/playdata/example1/N20160102S0423.fits
   Input tags: ['NON_SIDEREAL', 'AT_ZENITH', 'NIRI', 'NORTH', 'UNPREPARED', 'AZEL_TARGET', 'GEMINI', 'RAW', 'DARK', 'CAL']
   Input mode: sq
   Input recipe: makeProcessedDark
   Matched recipe: geminidr.niri.recipes.sq.recipes_DARK::makeProcessedDark
   Recipe location: /Users/klabrie/condaenvs/public3.10_3.1.0/lib/python3.10/site-packages/geminidr/niri/recipes/sq/recipes_DARK.py
   Recipe tags: {'NIRI', 'DARK', 'CAL'}
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

The last three "sq" recipes are really the "ql" recipes.  This a known
bug (circa April 2023).  The NIRI quicklook recipes are identical to
the science recipes and are just "Python imported" from the science module,
and that import trips the current implementation of ``showrecipes``.

.. code-block::

   showrecipes ../playdata/example1/N20160102S0270.fits --all

   geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
   geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.sq.recipes_IMAGE::reduce
   geminidr.niri.recipes.qa.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.qa.recipes_IMAGE::reduce
   geminidr.niri.recipes.sq.recipes_IMAGE::alignAndStack
   geminidr.niri.recipes.sq.recipes_IMAGE::makeSkyFlat
   geminidr.niri.recipes.sq.recipes_IMAGE::reduce

To see the content of a specific recipe, name it with the ``-r`` flag.

.. code-block::

   showrecipes ../playdata/example1/N20160102S0270.fits -r makeSkyFlat

Finally, to print the content of the quality assessment, "qa", recipe
named ``reduce`` (also the default in that ``qa`` library), use the
``-m`` flag:

.. code-block::

   showrecipes ../playdata/example1/N20160102S0270.fits -r reduce -m qa


Solutions to Primitives exercises
==================================================

.. _solution_primitives1:

Solution to :ref:`Exercise - Primitives 1 <ex_primitives1>`
-----------------------------------------------------------

The first step is get yourself familiarized with the primitive names.  This
can be done by looking at the recipe.

.. code-block::

   showrecipes ../playdata/example1/N20160102S0270.fits

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

   showpars ../playdata/example1/N20160102S0270.fits skyCorrect

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
   :emphasize-lines: 8

   showpars ../playdata/example1/N20160102S0270.fits darkCorrect

   Dataset tagged as {'NORTH', 'IMAGE', 'NIRI', 'UNPREPARED', 'SIDEREAL', 'GEMINI', 'RAW'}
   Settable parameters on 'darkCorrect':
   ========================================
    Name			Current setting

   do_cal               'procmode'           Calibration requirement
   Allowed values:
       procmode	Use the default rules set by the processingmode.
       force	Require a calibration regardless of theprocessing mode.
       skip	    Skip this correction, no calibration required.
       None	    Field is optional

   suffix               '_darkCorrected'     Filename suffix
   dark                 None                 Dark frame

The parameter ``do_cal`` controls the dark correction.  Set to "skip".

**Question 3:**

The last question is about the stacking of the reduced frames.  At the end
of the recipe there's the primitive ``stackFrames``.

.. code-block::

   showpars ../playdata/example1/N20160102S0270.fits stackFrames

   ...
   reject_method        'sigclip'            Pixel rejection method
   Allowed values:
       none	no rejection
       minmax	reject highest and lowest pixels
       sigclip	reject pixels based on scatter
       varclip	reject pixels based on variance array
   ...

The ``reject_method`` is the answer.  It can be set to ``minmax``, to ``none``,
to ``varclip``, or to ``sigclip`` (currently the default).


Solutions to the Explore data exercises
=======================================

.. _solution_explore1:

Solution to :ref:`Exercise - Explore 1 <ex_explore1>`
-----------------------------------------------------

**Question 1**

::

   reduce -r display N20160102S0271_stack.fits -p frame=2 threshold=None

Note that we did not use the ``display:`` prefix, like in ``display:frame=2``.
When the primitive name is not specified, the instruction applies to all
parameters with that name from any primitives in the recipe.  Here, we get
away with it because only the ``display`` primitive is being run.

**Question 2**

::

   reduce -r inspect @stdstar.lis -p pause=1



Solutions to the Local calibration manager exercises
====================================================

.. _solution_caldb1:

Solution to :ref:`Exercise - Caldb 1 <ex_caldb1>`
-------------------------------------------------
The ``dragonsrc`` file should look like this::

    [calibs]
    #databases = <where_the_data_package_is>/niriimg_tutorial/playground/cal_manager.db get
    databases = <where_the_data_package_is>/niriimg_tutorial/cal_manager.db get


``ls <where_the_data_package_is>/niriimg_tutorial`` should show a file called
``cal_manager.db``.  And ::

   caldb config

   Using configuration files: ('~/.dragons/dragonsrc',)

   /Users/klabrie/data/tutorials/niriimg_tutorial/cal_manager.db
     Type:  LocalDB
     Get:   True
     Store: False


   Database file: /Users/klabrie/data/tutorials/niriimg_tutorial/cal_manager.db

   The calibration dbs are all local, meaning that remote calibrations will not be downloaded


.. _solution_caldb2:

Solution to :ref:`Exercise - Caldb 2 <ex_caldb2>`
-------------------------------------------------
It is important do to successfully complete :ref:`Exercise - Caldb 1 <ex_caldb1>`
before attempting Exercise 2.

First confirm that the new calibration manager, the one ``niriimg_tutorial``
is active.

::

   caldb config

   Using configuration files: ('~/.dragons/dragonsrc',)

   /Users/klabrie/data/tutorials/niriimg_tutorial/cal_manager.db
     Type:  LocalDB
     Get:   True
     Store: False


   Database file: /Users/klabrie/data/tutorials/niriimg_tutorial/cal_manager.db

   The calibration dbs are all local, meaning that remote calibrations will not be downloaded

**Question 1**

::

   caldb add N20160102S0373_flat.fits

**Question 2**

::

   caldb list

   N20160102S0373_flat.fits       /data/workspace/niriimg_tutorial/playground

**Question 3**

Edit ``dragonsrc``.  Comment out the ``niriimg_tutorial`` path and uncomment
the ``playground`` path.

::

    [calibs]
    databases = <where_the_data_package_is>/niriimg_tutorial/playground/cal_manager.db get
    #databases = <where_the_data_package_is>/niriimg_tutorial/cal_manager.db get

Confirm activation with ``caldb config``.

::

   caldb list

   N20160102S0373_flat.fits       /data/workspace/niriimg_tutorial/playground
   N20160102S0423_dark.fits       /data/workspace/niriimg_tutorial/playground



Solutions to the ``reduce`` exercises
=====================================


.. _solution_reduce1:

Solution to :ref:`Exercise - "reduce" 1 <ex_reduce4>`
-----------------------------------------------------

::

   reduce -r detectSources N20160102S0296_stack.fits --suffix _ILoveDRAGONS

   Or

   reduce -r detectSources N20160102S0296_stack.fits --suffix=_ILoveDRAGONS


.. _solution_reduce2:

Solution to :ref:`Exercise - "reduce" 2 <ex_reduce2>`
-----------------------------------------------------

::

   reduce @flats.lis -p normalizeFlat:scale=mean --suffix _exercise2

.. _solution_reduce3:

Solution to :ref:`Exercise - "reduce" 3 <ex_reduce3>`
-----------------------------------------------------

::

   reduce @target.lis -r makeSkyFlat --suffix _skyflat


.. _solution_reduce4:

Solution to :ref:`Exercise - "reduce" 4 <ex_reduce4>`
-----------------------------------------------------

While it is not recommended to use a processed dark of the wrong exposure,
here is how you would force DRAGONS to use the science's master dark
on the flux standard from the demo.

::

    reduce @stdstar.lis -p --user_cal processed_dark:N20160102S0423_dark.fits


Solutions to the Customize recipes exercise
===========================================

.. _solution_customrecipe1:

Solution to :ref:`Exercise - Custom Recipe 1 <ex_customrecipe1>`
----------------------------------------------------------------

::

   showrecipes ../playdata/example1/N20160102S0363.fits

::

   cp /Users/klabrie/condaenvs/public3.10_3.1.0/lib/python3.10/site-packages/geminidr/niri/recipes/sq/recipes_FLAT_IMAGE.py .
   mv recipes_FLAT_IMAGE.py myNIRIflats.py

.. code-block:: python
   :emphasize-lines: 8

   p.prepare()
   p.addDQ()
   p.addVAR(read_noise=True)
   p.nonlinearityCorrect()
   p.ADUToElectrons()
   p.addVAR(poisson_noise=True)
   p.makeLampFlat()
   p.writeOutputs()
   p.normalizeFlat()
   p.thresholdFlatfield()
   p.storeProcessedFlat()
   p.display()

::

   reduce @flats.lis -r myNIRIflats.makeProcessedFlat



Solutions to the Tools exercise
===========================================

.. _solution_tools1:

Solution to :ref:`Exercise - Tools 1 <ex_tools1>`
-------------------------------------------------

::

   showd -d exposure_time,filter_name,ut_date ../playdata/example1/*.fits

   -----------------------------------------------------------------------------------
   filename                                   exposure_time   filter_name      ut_date
   -----------------------------------------------------------------------------------
   ../playdata/example1/N20160102S0270.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0271.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0272.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0273.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0274.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0275.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0276.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0277.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0278.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0279.fits          20.002       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0295.fits          10.005       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0296.fits          10.005       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0297.fits          10.005       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0298.fits          10.005       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0299.fits          10.005       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0363.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0364.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0365.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0366.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0367.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0368.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0369.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0370.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0371.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0372.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0373.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0374.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0375.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0376.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0377.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0378.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0379.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0380.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0381.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0382.fits          42.001       H_G0203   2016-01-02
   ../playdata/example1/N20160102S0423.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0424.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0425.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0426.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0427.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0428.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0429.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0430.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0431.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160102S0432.fits          20.002         blank   2016-01-02
   ../playdata/example1/N20160103S0463.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0464.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0465.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0466.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0467.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0468.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0469.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0470.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0471.fits           1.001         blank   2016-01-03
   ../playdata/example1/N20160103S0472.fits           1.001         blank   2016-01-03


.. _solution_tools2:

Solution to :ref:`Exercise - Tools 2 <ex_tools2>`
-------------------------------------------------

::

   dataselect ../playdata/example1/*.fits --expr='observation_class=="science" and observation_type=="OBJECT"' | showd -d object

   --------------------------------------------------
   filename                                    object
   --------------------------------------------------
   ../playdata/example1/N20160102S0270.fits   SN2014J
   ../playdata/example1/N20160102S0271.fits   SN2014J
   ../playdata/example1/N20160102S0272.fits   SN2014J
   ../playdata/example1/N20160102S0273.fits   SN2014J
   ../playdata/example1/N20160102S0274.fits   SN2014J
   ../playdata/example1/N20160102S0275.fits   SN2014J
   ../playdata/example1/N20160102S0276.fits   SN2014J
   ../playdata/example1/N20160102S0277.fits   SN2014J
   ../playdata/example1/N20160102S0278.fits   SN2014J
   ../playdata/example1/N20160102S0279.fits   SN2014J

::

   dataselect ../playdata/example1/*.fits --expr='observation_class=="partnerCal" and observation_type=="OBJECT"' | showd -d object

   -------------------------------------------------
   filename                                   object
   -------------------------------------------------
   ../playdata/example1/N20160102S0295.fits    FS 17
   ../playdata/example1/N20160102S0296.fits    FS 17
   ../playdata/example1/N20160102S0297.fits    FS 17
   ../playdata/example1/N20160102S0298.fits    FS 17
   ../playdata/example1/N20160102S0299.fits    FS 17
