.. _getIntroduction:

The GET Framework
=================

`Generalised Epithelial Transport (GET) <https://bitbucket.org/get>`_ is a project aimed at providing a user-friendly, web-based, computational modelling tool for epithelial cell transport, particularly in regard to the kidney. Recent presentations providing an overview of these tools are available:

* `doi: 10.6084/m9.figshare.870447 <http://dx.doi.org/10.6084/m9.figshare.870447>`_ (`International Conference on Biomedical Engineering <http://www.icbme.org/>`_ 2013)
* `doi: 10.6084/m9.figshare.947751 <http://dx.doi.org/10.6084/m9.figshare.947751>`_ (The 25th New Zealand Nephrology Conference, 2014)

With the GET framework we hope to provide a user-focused tool for investigations of epithelial transport across the spatial scales from molecular mechanisms through to collections of cells (*e.g.*, the renal nephron). Libraries of existing mathematical models and simulation experiments will be made available to users for the:

* investigation/application of previous studies;
* development of new studies based on previous work;
* validation of new or existing work with new experimental data or computational tools.

In developing the GET framework we have tried to take a modular approach, separating functional modules into their own reusable libraries or modules. Below we present the current list of modules in the GET framework and their current status.

.. contents::
  :backlinks: top

.. _getLibrary:

GET library
-----------

The GET library is the collection of `CellML <http://cellml.org>`_ models which form the library of reusable modules utilised by the GET tools to create modular epithelial cell model descriptions. `SED-ML <http://sed-ml.org>`_ will be used to provide descriptions of the simulation experiments to accompany the mathematical models in the library, to demonstrate specific instantiations of the models into simulation experiments. The library is available via both a `Physiome Repository workspace <http://models.physiomeproject.org/workspace/19f>`_ and `BitBucket <https://bitbucket.org/get/get-library>`_. The Physiome Repository workspace should be considered the primary source and we maintain the clone on BitBucket to take advantage of the range of `Mercurial <http://mercurial.selenic.com/>`_ management tools available on that platform. The ease with which we can do this demonstrates one of the advantages of using a distributed version control system like Mercurial.

The GET library currently consists of the core modules used by :ref:`GET creator <getCreator>`. The intention is to extend this library with a collection of curated CellML encodings of a range of epithelial transporters and whole cell models. The current collection of uncurated models can be seen in the `renal nephron <http://models.physiomeproject.org/exposure/42>`_ workspace.

.. _getCreator:

GET creator
-----------

`GET creator <https://bitbucket.org/get/get-creator>`_ is the tool within the GET framework which focuses on the automated creation (and potentially editing in the future) of epithelial cell models via the assembly of constituent components from the :ref:`GET library <getLibrary>`. In this manner, users are able to rapidly create customised epithelial cell models which capture the level of detail they require for their specific investigation. Through the incorporation of knowledge about the modules available in the library, GET creator is able to automatically define the *glue* which binds the generic modules together (*i.e.*, summing all fluxes of a particular molecule through a given membrane).

We aim to infer model encoding requirements from annotations of the models contained in the GET library, but currently GET creator has that knowledge embedded directly in the code. By shifting this knowledge to the model annotations, our approach will be extensible to any sufficiently annotated model that might be added to the library in the future. This will become important as the scope of the library extends beyond the current core models.

GET creator is designed as a library with a clearly defined programming interface. The `code repository <https://bitbucket.org/get/get-creator>`_ contains a demonstration application which makes use of this library to start the process of creating the model described by `Latta et al (1984)`_. While this test application is complete, the generated CellML model is not yet complete.

In addition to creating the CellML model configured for the users specific requirements, GET creator will also be able to customise generic simulation experiment descriptions from the GET library that are deemed suitable for the model being constructed. While certain basic simulation experiments that might be suitable can be inferred from the library models being integrated into the new cell model, further guidance from the user will result in a more comprehensive suite of simulation experiments being generated. In this manner, we envision the incorporation of some of the `functional curation <https://travis.cs.ox.ac.uk/FunctionalCuration/>`_ concepts developed for cardiac electrophysiology models could be transfered to epithelial cell transport models.

.. _getSimulator:

GET simulator
-------------

`GET simulator <https://bitbucket.org/get/get-simulator>`_ is the simulation engine for the GET framework. Underneath, GET simulator makes use of `CSim <http://cellml-simulator.googlecode.com>`_, a generic `CellML`_ simulation tool with both a command line client and a reusable library.

While whole epithelial cell models can be encoded in CellML and those models can be simulated by standard CellML-capable simulation tools, in most cases that will not be the full experiment the user desires to run. Instead, the method described by `Latta et al (1984)`_ has been implemented in GET simulator in order to solve for mass and charge conservation. The idea is that GET simulator makes use of CSim to evaluate parts of the model while following the `Latta et al (1984)`_ algorithm for solving the current state of the whole cell.

As mentioned above, `SED-ML`_ is used to encode the descriptions of the simulation experiments to be executed. SED-ML makes use of the `Kinetic Simulation Algorithm Ontology (KiSAO) <http://biomodels.net/kisao/>`_ to describe the numerical method to be used when executing a particular numerical simulation task. KiSAO does not currently contain suitable terms to describe the simulation algortihm implemented by GET simulator. So we are making use of a proposed extension for SED-ML for the inclusion of custom simulation algorithms. In this manner, we are able to encode our epithelial cell simulation experiments in generic SED-ML with all the advantages that that offers, but still have a method by which to inform other simulation engines that a specific algorithm is required. A suitable fall-back algortihm can also be defined such that simulation engines are able to perform the default integration of the cell model as mentioned above.

.. _getWebApp:

GET web application
-------------------

The GET web application is an attempt to provide a web-based drag-and-drop interface for assembling epithelial cell models. This interface makes use of the :ref:`getLibrary` and :ref:`getCreator` tools above via web services provided by the :ref:`getModelServer`. You can see a screenshot of the current state of the interface embedded in the :ref:`Physiome Modeller <physiome-modeller>` interface :ref:`here <embeddedGetWebApplication>`.

.. _getModelServer:

GET model server
----------------

The GET model server is a prototype web server providing access to the GET framework via standard web services. These services are specific to the GET framework, but as the project develops common tools will be extracted out as proposed features to be implemented as part of the software which runs the `Physiome Repository`_.

.. _Latta et al (1984): http://dx.doi.org/10.1007/BF01870733
.. _CellML: http://cellml.org
.. _SED-ML: http://sed-ml.org
.. _Physiome Repository: http://models.physiomeproject.org