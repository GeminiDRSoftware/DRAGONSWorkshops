.. solutions.rst

.. include:: DRAGONSlinks.txt

.. _solutions:

**********************
Solutions to exercises
**********************

Solutions to the Local calibration manager exercises
====================================================

.. _solution_caldb1:

Solution to :ref:`Exercise - Caldb 1 <ex_caldb1>`
-------------------------------------------------
The ``rsys.cfg`` file should look like this::

    [calibs]
    standalone = True

    #database_dir = <where_the_data_package_is>/niriimg_tutorial/playground
    database_dir = <where_the_data_package_is>/niriimg_tutorial


``ls <where_the_data_package_is>/niriimg_tutorial`` should show a file called
``cal_manager.db``.  And ::

   caldb config

   Using configuration file: ~/.geminidr/rsys.cfg
   Active database directory: <where_the_data_package_is>/niriimg_tutorial/
   Database file: <where_the_data_package_is>/niriimg_tutorial/cal_manager.db

   The 'standalone' flag is active, meaning that local calibrations will be used


.. _solution_caldb2:

Solution to :ref:`Exercise - Caldb 2 <ex_caldb2>`
-------------------------------------------------
It is important do to successfully complete :ref:`Exercise - Caldb 1 <ex_caldb1>`
before attempting Exercise 2.

First confirm that the new calibration manager, the one ``niriimg_tutorial``
is active.

::

   caldb config

   Using configuration file: ~/.geminidr/rsys.cfg
   Active database directory: <where_the_data_package_is>/niriimg_tutorial/
   Database file: <where_the_data_package_is>/niriimg_tutorial/cal_manager.db

   The 'standalone' flag is active, meaning that local calibrations will be used

**Question 1**

::

   caldb add N20160102S0373_flat.fits

**Question 2**

::

   caldb list

   N20160102S0373_flat.fits       /data/workspace/niriimg_tutorial/playground

**Question 3**

Edit ``rsys.cfg``.  Comment out the ``niriimg_tutorial`` path and uncomment
the ``playground`` path.

::

    [calibs]
    standalone = True

    database_dir = <where_the_data_package_is>/niriimg_tutorial/playground
    #database_dir = <where_the_data_package_is>/niriimg_tutorial

Confirm activation with ``caldb config``.

::

   caldb list

   N20160102S0373_flat.fits       /data/workspace/niriimg_tutorial/playground
   N20160102S0423_dark.fits       /data/workspace/niriimg_tutorial/playground



Solutions to the ``reduce`` exercises
=====================================

.. _solution_reduce1:

Solution to :ref:`Exercise - "reduce" 1 <ex_reduce1>`
-----------------------------------------------------

::

   reduce @flats.lis -p normalizeFlat:scale=mean --suffix _exercise1

.. _solution_reduce2:

Solution to :ref:`Exercise - "reduce" 2 <ex_reduce2>`
-----------------------------------------------------

::

   reduce @target.lis -r makeSkyFlat --suffix _skyflat


.. _solution_reduce3:

Solution to :ref:`Exercise - "reduce" 3 <ex_reduce3>`
-----------------------------------------------------

While it is not recommended to use a processed dark of the wrong exposure,
here is how you would force DRAGONS to use the science's master dark
on the flux standard from the demo.

::

    reduce @stdstar.lis --user_cal processed_dark:N20160102S0423_dark.fits


Solutions to the Customize recipes exercise
===========================================

.. _solution_customrecipe1:

Solution to :ref:`Exercise - Custom Recipe 1 <ex_customrecipe1>`
----------------------------------------------------------------

::

   showrecipes ../playdata/N20160102S0363.fits

::

   cp /Users/klabrie/condaenvs/gemini2_2.1.x_20200925/lib/python2.7/site-packages/dragons-2.1.1-py2.7-macosx-10.7-x86_64.egg/geminidr/niri/recipes/sq/recipes_FLAT_IMAGE.py .
   mv recipes_FLAT_IMAGE.py myNIRIflats.py

.. code-block:: python
   :emphasize-lines: 8,12

    def makeProcessedFlat(p):

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

   showd -d exposure_time,filter_name,ut_date ../playdata/*.fits

   --------------------------------------------------------------------------
   filename                          exposure_time   filter_name      ut_date
   --------------------------------------------------------------------------
   ../playdata/N20160102S0270.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0271.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0272.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0273.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0274.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0275.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0276.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0277.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0278.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0279.fits          20.002       H_G0203   2016-01-02
   ../playdata/N20160102S0295.fits          10.005       H_G0203   2016-01-02
   ../playdata/N20160102S0296.fits          10.005       H_G0203   2016-01-02
   ../playdata/N20160102S0297.fits          10.005       H_G0203   2016-01-02
   ../playdata/N20160102S0298.fits          10.005       H_G0203   2016-01-02
   ../playdata/N20160102S0299.fits          10.005       H_G0203   2016-01-02
   ../playdata/N20160102S0363.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0364.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0365.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0366.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0367.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0368.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0369.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0370.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0371.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0372.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0373.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0374.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0375.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0376.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0377.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0378.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0379.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0380.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0381.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0382.fits          42.001       H_G0203   2016-01-02
   ../playdata/N20160102S0423.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0424.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0425.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0426.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0427.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0428.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0429.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0430.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0431.fits          20.002         blank   2016-01-02
   ../playdata/N20160102S0432.fits          20.002         blank   2016-01-02
   ../playdata/N20160103S0463.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0464.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0465.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0466.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0467.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0468.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0469.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0470.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0471.fits           1.001         blank   2016-01-03
   ../playdata/N20160103S0472.fits           1.001         blank   2016-01-03


.. _solution_tools2:

Solution to :ref:`Exercise - Tools 2 <ex_tools2>`
-------------------------------------------------

::

   dataselect ../playdata/*.fits --expr='observation_class=="science" and observation_type=="OBJECT"' | showd -d object

   -----------------------------------------
   filename                           object
   -----------------------------------------
   ../playdata/N20160102S0270.fits   SN2014J
   ../playdata/N20160102S0271.fits   SN2014J
   ../playdata/N20160102S0272.fits   SN2014J
   ../playdata/N20160102S0273.fits   SN2014J
   ../playdata/N20160102S0274.fits   SN2014J
   ../playdata/N20160102S0275.fits   SN2014J
   ../playdata/N20160102S0276.fits   SN2014J
   ../playdata/N20160102S0277.fits   SN2014J
   ../playdata/N20160102S0278.fits   SN2014J
   ../playdata/N20160102S0279.fits   SN2014J

::

   dataselect ../playdata/*.fits --expr='observation_class=="partnerCal" and observation_type=="OBJECT"' | showd -d object

   ----------------------------------------
   filename                          object
   ----------------------------------------
   ../playdata/N20160102S0295.fits    FS 17
   ../playdata/N20160102S0296.fits    FS 17
   ../playdata/N20160102S0297.fits    FS 17
   ../playdata/N20160102S0298.fits    FS 17
   ../playdata/N20160102S0299.fits    FS 17
