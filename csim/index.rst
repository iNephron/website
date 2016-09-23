.. _csimIntroduction:

CSim - a tool for executing CellML models
=========================================

The goal of this project is to produce a minimal stand-alone library that generates executable functions for `CellML <http://cellml.org>`_ models. We aim to support Linux, Mac, and Windows and to attempt to make it relatively easy for simulation tool developers to add CellML support. A further aim is to have as few (if any) dependencies on external tools or libraries that a user must also install in order to use this simulation tool. 

This tool is based on the original `CellMLSimulator <http://cellml.sourceforge.net>`_ `[1] <http://dx.doi.org/10.1093/bioinformatics/btn080>`_ with all the extra guff from that tool stripped out, leaving just the bare essentials required for simulation.

CSim code is hosted on GitHub with the primary repository: https://github.com/nickerso/csim. This website is available at: https://get.readthedocs.io.

.. .. warning::
.. 
..    Most of the documentation currently found here is a bit behind some recent developments in CSim. Notably, with `Google Code bidding farewell <http://google-opensource.blogspot.com/2015/03/farewell-to-google-code.html>`_, the primary repository for the CSim code has moved to `GitHub <http://github.com>`_, and can currently be found here: https://github.com/nickerso/csim.

.. toctree::
   :maxdepth: 1
   :titlesonly:

   building-csim
   usingLibCsim

.. toctree::
   :hidden:
   
   release-notes
   usingCLI
   summary-description
