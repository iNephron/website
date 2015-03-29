.. _csimHowToBuild:

Building CSim
=============

.. todo::

   Original doc from google code, need to update for recent changes in the build of CSim.

Some initial documentation on how to build CSim, this is a work in progress which may not always be the most up-to-date source of information. I'll try to keep it updated as things go. At present, my primary development machine is a MacBook Pro running OS X 10.10. I also use a virtual Ubuntu 14.04 (x86_64) and virtual Windows 7 (Visual Studio 2013 32bit builds) to test things out.

Details
-------

External dependencies
+++++++++++++++++++++

Compiler
********

I'm using a lot of new'ish C++11 code, so you need a compiler that will handle it all. This is clang on OS X, gcc 4.8+ on Linux, and Visual Studio 2012+ on Windows (I'm using Visual Studio 2013 currently.)

CMake
*****

I'm using `CMake <http://cmake.org>`_ for configuring the build system and most of the required dependencies also build with CMake. You should be able to install CMake on most systems fairly easily. Version 2.8 or higher is required for CSim.

The CellML API
**************

The Auckland implementation of the `CellML API <http://cellml-api.sourceforge.net/>`_ is required to build CSim. The easiest way to get this is via the binary SDK, but its getting easier to build it yourself using CMake if you really want to. Grab the appropriate CellML SDK for your platform from http://cellml-api.sourceforge.net/. Currently there are issues with the latest release of the CellML API (1.12), so I am building my own version on each platform from the current trunk code. I'll use ``$CellML-SDK`` to refer to the folder to which you extract the binary SDK (or have installed your own builds of the API).

CVODES from Sundials
********************

CSim uses the CVODES integrator from `Sundials <https://computation.llnl.gov/casc/sundials/main.html>`_. This is generally available as a system package under Linux. On my MacBook I installed it using the `Homebrew <http://mxcl.github.com/homebrew/>`_ (i.e., ``$> brew install sundials``). However, in order to limit dependencies on packages that users may not have (especially since these system packaged introduce dependencies on BLAS and Lapack libraries), we typically link against our own static builds of CVODES. If you are building CSim for yourself and have CVODES available on your system you can skip this step.

Under windows 7 I downloaded the `CVODES 2.7.0 release source code <https://computation.llnl.gov/casc/sundials/download/download.html>`_, extracted it, and used CMake to build it with commands like (in a MSVC command prompt)::

   $> mkdir %CSIM%/third_party/cvodes
   $> cd %CSIM%/third_party/cvodes
   $> ...extract downloaded cvodes archive...
   $> mkdir build/win32
   $> cd build/win32
   $> cmake -DCMAKE_BUILD_TYPE=Release -G "NMake Makefiles" -DCMAKE_INSTALL_PREFIX=%CSIM%/third_party/win32 ../../cvodes-2.7.0
   $> nmake install

Under Mac OS X and Linux, the same commands work fine, just use the appropriate system folder (macosx or linux) and the default makefiles based configuration is fine so you don't need to specify the ``-G`` option. For linux, you need to add ``-fPIC`` to your compile flags to build the library version of CSim.

LibXML2
*******

Standard system library on Mac and Linux. On windows, I downloaded binaries from http://www.zlatkovic.com/libxml.en.html (found via the official `xmlsoft.org downloads <http://xmlsoft.org/downloads.html>`_) and extracted them into the same ``%CSIM%/third_party/win32`` folder as CVODES was installed to above.

zlib
****

Standard system library on Mac and Linux. Comes as part of the libxml2 binaries above for Windows.

LLVM/Clang
**********

The CellML API can be used to generate C code for a given CellML model. In CSim, I use LLVM and Clang to compile the generated C code and execute it, all in memory. Thus removing the need for the user to have a C compiler on their system.

NOTE: CSim now uses LLVM/Clang version 3.4.1 - the binary downloads available from http://llvm.org/releases under OS X and Linux. The Windows binary download just seems to be the actual clang toolchain rather than all the libraries and include files needed to link clang into CSim. See the build instructions here: http://clang.llvm.org/get_started.html. A normal CMake build under Windows seems to work just fine - although takes a long time on my little virtual machine. Perhaps things would be quicker using an actual Visual Studio solution as that would allow multi-processing the build (but my virtual machine is quite limited so I stick to nmake for now).

::

   $> mkdir %CSIM%\third_party\LLVM+Clang
   $> cd %CSIM%\third_party\LLVM+Clang
   ...extract downloaded llvm-3.4.1.src.tar.gz here...
   $> cd llvm-3.4.1.src\tools
   ...extract clang-3.4.1.src.tar.gz here...
   $> ren clang-3.4.1.src clang
   $> cd %CSIM%\third_party\LLVM+Clang
   $> mkdir build
   $> cd build
   $> cmake -DCMAKE_BUILD_TYPE=Release -G "NMake Makefiles" -DCMAKE_INSTALL_PREFIX=%CSIM%/third_party/win32 -DLLVM_BUILD_TESTS=FALSE ..\llvm-3.4.1.src

When configuring CSim, just point LLVM_INSTALL_DIR at the place you installed LLVM+Clang and everything should just work.

Keep an eye out for this issue, that popped up when building LLVM with MSVC 2008, but not MSVC 2010 Express. Since I don't have permissions on my build machine to try the suggested work-arounds, I simply kept restarting the build each time the error came up and it managed to (eventually) get through to completion.

PCRE
****

TODO :)

Configure
+++++++++

I'm using CMake to configure the build. In general, something like this should work::

   $> hg clone https://code.google.com/p/cellml-simulator CSim
   $> cd CSim
   $> mkdir -p build/release
   $> cd build/release
   $> cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=$CellML-SDK -DLLVM_INSTALL_DIR=$LLVM_DIR -DCMAKE_INSTALL_PREFIX=path/to/install/csim ../..

You need to point to the installed location of the CellML API so that the libraries and header files can be found. I have recently found that on my MacBook I need to add /usr/lib to the CMAKE_LIBRARY_PATH with -DCMAKE_LIBRARY_PATH="/usr/lib;$CellML-SDK/lib" to make sure the the correct version of libXml2 is found (otherwise its picking up one from mono which doesn't seem to play nice with the API).

If you have built your own LLVM using cmake, you can set USE_LLVM_RELEASE to false to enable the use of the LLVM cmake build config in setting up the CSim build system. But as of the 3.1 release, such information isn't included in the released files so I have manually set up the required flags, includes, and libraries needed by CSim based on the LLVM_INSTALL_DIR. Also, when using the win32 build of LLVM from above, I am still using the manually configured LLVM.

For windows builds, you need to add %CSIM%/third_party/win32 to the CMAKE_PREFIX_PATH so that the extra non-system dependencies are found on that platform. This is also true if you built your own CVODES under Mac or Linux.

Build
+++++

Once you have successfully configured, you can build CSim with a simple::

   $> make

or, on windows::

   $> nmake

Install
+++++++

Following a successful build, it is useful to install the built application. This has the benefit of tidying up all the required libraries that are not statically linked into CSim and collecting them into appropriate locations depending on the OS you are building on. You can override the default install location with the CMAKE_INSTALL_PREFIX as per the configuration above. You can also specify the DESTDIR on the install to override the configured install location.

Linux
*****

::

   $> make install
   ... or ..
   $> make install DESTDIR=path/to/install

will result in:

* path/to/install/bin
   
   * csim - the CSim command line application
   * lib*.so - the required non-static libraries for CSim (currently just those from the CellML API)

and the installed csim application with be configured to find the required libraries from that directory.

Mac OSX
*******

::

   $> make install
   ... or ...
   $> make install DESTDIR=path/to/install

will result in:

* path/to/install/
   
   * csim.app - the CSim application bundle (contains all required files)
   * Contents/MacOS/csim - the CSim command line application
   * Contents/MacOS/lib*.dylib - the required non-static libraries for CSim (currently just those from the CellML API)

and the installed csim application with be configured to find the required libraries from that application bundle.

Windows
*******

::

   $> nmake install
   ... or ...
   $> nmake install DESTDIR=path/to/install

will result in:

* path/to/install/
   * bin
   * csim.exe - the CSim command line application
   * \*.dll - the required non-static libraries for CSim (currently those from the CellML API and libXML2)

and the installed csim.exe application with be configured to find the required libraries from bin folder.

Packaging
+++++++++

With CMake it is relatively straightforward to also package up the application for distribution to other machines. CPack makes use of the installation set-up in the CMake configuration to create packages. I haven't played with this too much, but the following produces the packages that I currently make available, all run from the folder in which you build CSim:

* Linux: ``$> cpack -G TGZ`` - creates a .tar.gz archive;
* Mac OSX: ``$> cpack -G TGZ`` - creates a .tar.gz archive (DragNDrop doesn't seem to work with my combination of CMake and XCode versions, a newer CMake might fix this http://public.kitware.com/Bug/view.php?id=13003);
* Windows: ``$> cpack -G ZIP`` - creates a zip file.

I currently make a source package with the command::

   >$ hg archive --rev v0.4.2 csim-0.4.2-src.tar.gz
   