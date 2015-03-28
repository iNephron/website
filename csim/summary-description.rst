.. _csimSummaryDescription:

CSim Summary Description
========================

CSim expects a summary description of the numerical simulation task to perform to be found in the CellML model it is given to simulate. It is generally expected that SED-ML processing tools would insert such a summary when pre-processing models to ready them for computation, and that is how the library version of CSim works.

Details
-------

The summary description should be in the CellML document provided to CSim for computation and is expected to be found in a ``<simulation>`` element in the namespace ``http://cellml.sourceforge.net/csim/simulation/0.1#`` (the simulation can be anywhere in the document). An example demonstrating the use of this summary description based on a SED-ML document is available, with the summary description shown below.

::

   <simulation xmlns="http://cellml.sourceforge.net/csim/simulation/0.1#" id="simulation1">
           <boundVariable start="0.0" end="6.283185307179586232" maxStep="0.1" tabulationStep="0.7"/>
           <outputVariable component="main" variable="x" column="1"/>
           <outputVariable component="main" variable="sin1" column="2"/>
           <outputVariable component="main" variable="sin2" column="3"/>
           <outputVariable component="main" variable="sin3" column="4"/>
    </simulation>

The summary description simply states the domain over which the model should be computed, the ``<boundVariable>`` element. In this example the user wants to compute the interval from ``0`` to ``2*pi`` with a maximum step of ``0.1`` and saving results in steps of ``0.7``. The only other information currently required is a listing of the variables from the model that the user requires on the tabulation grid described by the bound variable description. This consists of the component in which the variable can be found, and the name of the variable in that component (component + variable name uniquely identifies every variable in a CellML model), and the column to output that variable in the tabulation grid.