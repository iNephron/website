.. _csimIntroduction:

CSim - a tool for executing CellML models
=========================================

The goal of this project is to produce a minimal stand-alone simulation tool for `CellML <http://cellml.org>`_ models that works on Linux, Mac, and Windows. It is intended that a tool like this is most likely used as a backend simulator (as part of a SED-ML tool). The aim is to have as few (if any) dependencies on external tools or libraries that a user must also install in order to use this simulation tool. We currently distribute CSim as a:

* command line executable which uses its own :ref:`custom annotations <csimSummaryDescription>` in a CellML model to define the simulation (which will likely be deprecated in favour of :ref:`getSimulator` at some point); and
* a C++ library.

This tool is based on the original `CellMLSimulator <http://cellml.sourceforge.net>`_ `[1] <http://dx.doi.org/10.1093/bioinformatics/btn080>`_ with all the extra guff from that tool stripped out, leaving just the bare essentials required for simulation.

.. toctree::
   :maxdepth: 1
   :titlesonly:

   summary-description
