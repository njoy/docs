.. _metaconfigureInstall:

#############################
Installing with metaconfigure
#############################

These instructions are for building NJOY21 prior to version 1.2.0. More up to date instructions can be found on the :ref:`install <install>` page.

Both `NJOY21 <https://github.com/njoy>`__ and
`NJOY2016 <https://github.com/njoy>`__ use the same configuration and
build process. Additionally, all of the supporting
`components </Components.html>`__ use the same process.

For the impatient
~~~~~~~~~~~~~~~~~

.. code:: bash

   # Download the source code
   git clone https://github.com/njoy/NJOY21.git

   # Get the desired version of NJOY21 (1.1.0 in this example)
   cd NJOY21
   wget https://raw.githubusercontent.com/njoy/signatures/master/NJOY21/1.1.0-NJOY21.json
   ./metaconfigure/fetch_subprojects.py 1.1.0-NJOY21.json

   # Configure the build process
   mkdir bin
   cd bin
   cmake -D fetched_subprojects=true ../

   # Build NJOY1
   make

   # Test NJOY1
   make test

Prerequisites
-------------

Development for NJOY uses the latest published language standards that
are widely supported by compiler vendors, *at the start of development*.
Therefore, the minimum language standards are

-  C++17 or higher
-  Fortran 2003 or higher
-  `Python 3.4+ <https://python.org/>`__

Additionally, we use `CMake <https://cmake.org/>`__ to configure the
build system and `git <https://git-scm.com>`__ for version control.

-  `CMake 3.2+ <https://cmake.org/>`__
-  `git 2.1+ <https://git-scm.com>`__

Build Process
-------------

Those interested in the NJOY21 development version should `read
here <developers.html>`__ for slightly different build instructions.

Downloading
~~~~~~~~~~~

To download NJOY21, simply ``git clone`` the repository. First move into
the directory where you want the source code, then execute:

.. code:: bash

   git clone https://github.com/njoy/NJOY21.git

In addition, you should download a signature file for the version of
NJOY21 for which you are interested. The signatures can be found in our
repository on GitHub at
`https://github.com/njoy/signatures/NJOY21 <https://github.com/njoy/signatures/tree/master/NJOY21>`__.
For this example we will use the signature file:
`1.1.0-NJOY21.json <https://raw.githubusercontent.com/njoy/signatures/master/NJOY21/1.1.0-NJOY21.json>`__.
Save this file inside the NJOY21 directory that was created during the
``git clone`` operation.

.. code:: bash

   wget https://raw.githubusercontent.com/njoy/signatures/master/NJOY21/1.1.0-NJOY21.json

You can inspect the json file before downloading it by looking at the
`signatures repository <https://github.com/njoy/signatures>`__ on
GitHub.

Similarly for NJOY2016

.. code:: bash

   git clone https://github.com/njoy/NJOY2016.git

Configuring
~~~~~~~~~~~

The configuration and build is performed in a directory (``bin``) inside
the source directory

.. code:: bash

   cd NJOY21
   ./metaconfigure/fetch_subprojects.py ../1.1.0-NJOY21.json
   mkdir bin
   cd bin
   cmake -D fetched_subprojects=true ../

Note this will require a connection to the internet as ``cmake`` command
will download (``clone``) the necessary dependencies. These will be
placed in the ``dependencies`` directory.

Compiling/Building
~~~~~~~~~~~~~~~~~~

.. code:: bash

   make

You can provide the command-line option ``-j n`` (where ``n`` is the
number of concurrent/parallel “jobs”) to speed up the compilation step.

Testing
~~~~~~~

.. code:: bash

   make test

Like for the compiling step, you can pass the ``-j`` command-line option
here as well.

If all the tests pass, you should see something that looks like:

::

   100% tests passed, 0 tests failed out of 90

Building Offline
~~~~~~~~~~~~~~~~

For this configuration/build process, a connection to the internet is
required as subprojects are downloaded from GitHub. There are many
instances when one would need to build NJOY on a machine that is not
connected to the internet. To do that, please use these steps:

.. code:: bash

   # Download the source code
   git clone https://github.com/njoy/NJOY21.git

   # Configure the build process
   cd NJOY21
   ./metaconfigure/fetch_subprojects.py ../1.1.0-NJOY21.json

   #### Transfer the entire NJOY21 directory to machine 
   #### not connected to the internet

   cd NJOY21
   mkdir bin
   cd bin
   cmake -D fetched_subprojects=TRUE ../

   # Build NJOY21
   make

   # Test NJOY21
   make test

Compliant Compilers
-------------------

We have tested NJOY2016 and NJOY21 with the following compilers.

C++17 Compliant Compiler
~~~~~~~~~~~~~~~~~~~~~~~~

-  **Mac:**

   -  The clang compiler that ships with XCode will be sufficient

-  **Linux:**

   -  `gcc 7 <https://gcc.gnu.org>`__

-  **Windows:**

   -  `clang 5 <http://llvm.org>`__
   -  `gcc 7 <https://gcc.gnu.org>`__

   Note that in Windows we currently support compiling and running under
   the `Windows Subsystem for
   Linux <https://docs.microsoft.com/en-us/windows/wsl/about>`__ or the
   Cygwin environment. If you have the necesssary components installed,
   this *should* also work in the DOS environment.

Fortran 2003 Compliant Compiler
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  **Linux:**

   -  `gfortran <https://gcc.gnu.org/fortran/>`__ This is included as
      part of the standard `gcc <https://gcc.gnu.org>`__ suite of
      compilers.

-  **Mac:**

   -  Installing gcc 7 via `homebrew <https://brew.sh>`__ or
      `spack <https://spack.io>`__ works quite well.

-  **Windows:**

   -  `gfortran <https://gcc.gnu.org/fortran/>`__ This is included as
      part of the standard `gcc <https://gcc.gnu.org>`__ suite of
      compilers.

Other compilers will most likely compile without any problems, but may
give different answers. The differences are typically small and due to
different ways of optimizing the code from each compiler.
