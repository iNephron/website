.. _csimUsingCLI:

CSim Command Line Interface
===========================

.. todo::

   Original doc from google code move. Need to update for current version of CSim.

CSim is currently able to be built as a command line application. Once you have either grabbed a pre-built CSim package or built it yourself, you are ready to see if it works.

Details
-------

The easiest way to run CSim is to run either a pre-built binary or one that you have built and installed yourself. This way the application will be able to find all its required libraries (at least the ones that are not easy to install/commonly available on each of the supported platforms). If you install CSim at the location ``CSIM`` then the following is a simple test to make sure it works:

On Mac OSX::

   $> $CSIM/csim.app/Contents/MacOS/csim http://models.cellml.org/w/andre/VPH-MIP/@@rawfile/607ac903e04cdcb0ab58b50c9c10677c2bd06b32/experiments/periodic-stimulus.xml

or Linux::

   $> $CSIM/bin/csim http://models.cellml.org/w/andre/VPH-MIP/@@rawfile/607ac903e04cdcb0ab58b50c9c10677c2bd06b32/experiments/periodic-stimulus.xml

and Windows::

   $> %CSIM%/bin/csim.exe http://models.cellml.org/w/andre/VPH-MIP/@@rawfile/607ac903e04cdcb0ab58b50c9c10677c2bd06b32/experiments/periodic-stimulus.xml

will run the Hodgkin & Huxley model and write out the results to the screen if all goes well.

Running from the build folder
-----------------------------

If you want to run CSim directly in the build folder without installing it, you need to make sure it can find all the required libraries.

For example, on Mac OSX::

   >$ export DYLD_LIBRARY_PATH=$CellML-SDK/lib

and on Linux::

   $> export LD_LIBRARY_PATH=$CellML-SDK/lib
   
and on Windows, its easiest just to copy all required libraries into the build folder, but maybe something with the ``PATH`` is what you need to do?