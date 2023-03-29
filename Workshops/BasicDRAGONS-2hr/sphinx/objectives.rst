.. objectives.rst

.. _objectives:

*******************
Learning Objectives
*******************

This material is built for a 2-hr workshop, planning on 90 minutes for the
workshop and 30 minutes for questions and helping participants.

In this workshop, the participants will gain a basic understanding about the
following topic:

* What is DRAGONS, what it isn't.
* What is a typical DRAGONS workflow.
* What are the principal components of DRAGONS.
* What is a recipe, a recipe library, and a primitive.
* How to do coarse inspection of the data.
* How to use the calibration manager.
* How to customize input parameters and recipes.
* How to control ``reduce``.
* What utility tools are available and how to use them.

This workshop tries to cover a lot in a short period of time.  The focus will
be on some key, often needed elements.  The participants should refer to the
`DRAGONS documentation <http://dragons.readthedocs.io/en/stable>`_ for more
in-depth information.

This workshop uses the command-line interface to DRAGONS.  An API is available
but will not be covered in this workshop.

Setting up
==========
The participants who wish to following along and run the examples and
exercises are expected to have already installed DRAGONS on their
computer.  See the **installation instructions**, |RSUserInstall|.

Follow the Python 3 and DRAGONS instructions, IRAF is not needed.  Make sure
that you do the steps in the "Configure DRAGONS" section.

.. important:: **This workshop is written for DRAGONS v3.1**

The participants will need to download the data package to run the examples
and the exercises:

`<http://www.gemini.edu/sciops/data/software/datapkgs/niriim_tutorial_datapkg-extended-v1.tar>`_

Download it and unpack it somewhere convenient.

.. highlight:: bash

::

    cd <somewhere convenient>
    tar xvf niriim_tutorial_datapkg-extended-v1.tar
    bunzip2 niriimg_tutorial/playdata/example1/*.bz2

The datasets are found in the subdirectory ``niriimg_tutorial/playdata/example1/``, and
we will work in the subdirectory named ``niriimg_tutorial/playground``.

.. note::
   If you are attending a live workshop, it is advisable to also download the
   preprocessed data package in case you encounter technical difficulties
   that prevents you from running the full demo yourself.
   The content of that package will allow you to continue with most of the exercises using
   those preprocessed data.  Download the "just-in-case" package at:

   `<https://drive.google.com/file/d/1sJd8bHUFZ3-dNVT3_VllcqplnoSZr4LJ/view?usp=sharing>`_

   You do not need to do anything with it now other then keeping it somewhere
   safe. Hopefully, everything will run smoothly and you will not need it.