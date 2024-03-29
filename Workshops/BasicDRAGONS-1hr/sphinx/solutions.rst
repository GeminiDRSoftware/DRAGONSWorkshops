.. solutions.rst

.. .. include:: DRAGONSlinks.txt

.. _basic1_solutions:

**********************
Solutions to exercises
**********************

Solutions to the Local calibration manager exercises
====================================================

.. _basic1_solution_caldb1:

Solution to :ref:`Exercise - Caldb 1 <basic1_ex_caldb1>`
--------------------------------------------------------
The ``dragonrc`` file should look like this::

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


.. _basic1_solution_caldb2:

Solution to :ref:`Exercise - Caldb 2 <basic1_ex_caldb2>`
--------------------------------------------------------
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

.. _basic1_solution_reduce1:

Solution to :ref:`Exercise - "reduce" 1 <basic1_ex_reduce1>`
------------------------------------------------------------

::

   reduce @flats.lis -p normalizeFlat:scale=mean --suffix _exercise1

.. _basic1_solution_reduce2:

Solution to :ref:`Exercise - "reduce" 2 <basic1_ex_reduce2>`
------------------------------------------------------------

::

   reduce @target.lis -r makeSkyFlat --suffix _skyflat


.. _basic1_solution_reduce3:

Solution to :ref:`Exercise - "reduce" 3 <basic1_ex_reduce3>`
------------------------------------------------------------

While it is not recommended to use a processed dark of the wrong exposure,
here is how you would force DRAGONS to use the science's master dark
on the flux standard from the demo.

::

    reduce @stdstar.lis --user_cal processed_dark:N20160102S0423_dark.fits


Solutions to the Customize recipes exercise
===========================================

.. _basic1_solution_customrecipe1:

Solution to :ref:`Exercise - Custom Recipe 1 <basic1_ex_customrecipe1>`
-----------------------------------------------------------------------

::

   showrecipes ../playdata/example1/N20160102S0363.fits

::

   cp /Users/klabrie/condaenvs/public3.10_3.1.0/lib/python3.10/site-packages/geminidr/niri/recipes/sq/recipes_FLAT_IMAGE.py .
   mv recipes_FLAT_IMAGE.py myNIRIflats.py

.. code-block:: python
   :emphasize-lines: 10,14

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

.. _basic1_solution_tools1:

Solution to :ref:`Exercise - Tools 1 <basic1_ex_tools1>`
--------------------------------------------------------

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


.. _basic1_solution_tools2:

Solution to :ref:`Exercise - Tools 2 <basic1_ex_tools2>`
--------------------------------------------------------

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
