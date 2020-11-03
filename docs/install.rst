.. _install:

###############
Installing NJOY
###############

Both `NJOY21 <https://github.com/njoy>`__ and `NJOY2016 <https://github.com/njoy>`__ use the same configuration and build process. Additionally, all of the supporting `components </Components.html>`__ use the same process. The steps on this page are for building NJOY21, but are equally applicable to NJOY2016.

For the impatient
=================

.. code:: bash

   # Download the latest version of the code
   git clone --branch v1.2.0 https://github.com/njoy/NJOY21.git

   cd NJOY21

   # Configure the build process
   mkdir bin
   cd bin

   # Build NJOY21
   make

   # Test NJOY21
   make test

Prerequisites
=============

Development for NJOY uses the latest published language standards that are widely supported by compiler vendors, *at the start of development*. Therefore, the minimum language standards are

-  C++17 or higher
-  Fortran 2003 or higher
-  `Python 3.4+ <https://python.org/>`__

Additionally, we use `CMake <https://cmake.org/>`__ to configure the
build system and `git <https://git-scm.com>`__ for version control.

-  `CMake 3.2+ <https://cmake.org/>`__
-  `git 2.1+ <https://git-scm.com>`__

Build Process
=============

Downloading
~~~~~~~~~~~

To download NJOY21, simply ``git clone`` the repository. First move into the directory where you want the source code, then execute:

.. code:: bash

   git clone https://github.com/njoy/NJOY21.git

This will get the latest version of the code. If you are interested in a particular version, you need to specify the git branch corresponding to that version like this for version 1.2.0:

.. code-block:: console

   git clone --branch v1.2.0 https://github.com/njoy/NJOY21.git

Please note that getting versions of NJOY21 prior to version 1.2.0 was different---and more convoluted. For information on how to do that see :ref:`this page <metaconfigureInstall>`.



Configuring
~~~~~~~~~~~

The configuration and build is performed in a directory (``bin``) inside
the source directory

.. code:: bash

   cd NJOY21
   mkdir bin
   cd bin
   cmake ../

Note this will require a connection to the internet as ``cmake`` command will download the necessary dependencies. 

Compiling/Building
~~~~~~~~~~~~~~~~~~

.. code:: bash

   make

You can provide the command-line option ``-j n`` (where ``n`` is the number of concurrent/parallel “jobs”) to speed up the compilation step.

Testing
~~~~~~~

.. code:: bash

   make test

Like for the compiling step, you can pass the ``-j`` command-line option here as well.

If all the tests pass, you should see something that looks like:

::

   100% tests passed, 0 tests failed out of 90

Building Offline
~~~~~~~~~~~~~~~~

For this configuration/build process, a connection to the internet is required as subprojects are downloaded from GitHub. There are many instances when one would need to build NJOY on a machine that is not connected to the internet. To do that, please use these steps:

.. code:: bash

   # Download the source code
   git clone https://github.com/njoy/NJOY21.git

   # Configure the build process
   cd NJOY21

   #### Transfer the entire NJOY21 directory to machine 
   #### not connected to the internet

   cd NJOY21
   mkdir bin
   cd bin

   # Build NJOY21
   make

   # Test NJOY21
   make test

Compliant Compilers
-------------------

We have tested NJOY2016 and NJOY21 with the following compilers.

C++17 Compliant Compiler
~~~~~~~~~~~~~~~~~~~~~~~~
We *strongly* suggest building with the LLVM C++ compiler. We have found this compiler to be *much* faster in compiling as well as being fast at runtime.

-  **Mac:**

   -  LLVM 9.0.1. The version that comes with XCode will be sufficient.

-  **Linux:**

   -  `gcc 8.3 <https://gcc.gnu.org>`__

   Note that in Windows we currently support compiling and running under the `Windows Subsystem for Linux <https://docs.microsoft.com/en-us/windows/wsl/about>`_

Fortran 2003 Compliant Compiler
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  **Linux:**

   -  `gfortran <https://gcc.gnu.org/fortran/>`_ This is included as part of the standard `gcc <https://gcc.gnu.org>`_ suite of compilers.

-  **Mac:**

   -  Installing gcc 8.3 via `homebrew <https://brew.sh>`_ or `spack <https://spack.io>`_ works quite well.

Other compilers will most likely compile without any problems, but may give different answers. The differences are typically small and due to different ways of optimizing the code from each compiler. The differences are not likely to be significant.
