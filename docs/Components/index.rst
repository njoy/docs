NJOY21 Components
=================
This page serves to document the various components that make up NJOY21.

NJOY Modules
-------------------
The biggest components of NJOY21 are called *modules*. These modules are those that replicate the functionality of Legacy [#Legacy]_ NJOY modules. Listed below are the modules that have been modernized and updated. The documentation for each is given in their respective locations. If a module is not listed here, please see the documentation for `Legacy NJOY <https://github.com/njoy/NJOY2016-manual/raw/master/njoy16.pdf>`_.

`RECONR <https://docs.njoy21.io/projects/RECONR>`_
   Resonance reconstruction

Modern NJOY Components
----------------------
There are many components that make up NJOY21, or that make up one of it's modules. These components can be used independently or integrated with others. Listed here are just a few of the components. All of our components can be found on our GitHub page: `https://github.com/njoy <https://github.com/njoy>`_.

`ENDFtk <https://github.com/njoy/ENDFtk>`_
   Toolkit for working with ENDF-formatted files.

`resonanceReconstruction <https://github.com/njoy/resonanceReconstruction>`_
   Perform resonance reconstruction calculations for these formalisms

   - Single-level Breit-Wigner,
   - Multi-level Breit-Wigner,
   - Reich-Moore,
   - R-Matrix Limited.

   resonanceReconstruction can also reconstruct unresolved resonances from the following ENDF formats:

   - Energy-independent (Case A),
   - Energy-independent Fission Widths (Case B),
   - Energy-dependent (Case C).

`interpolation <https://github.com/njoy/interpolation>`_
   Library for interpolating one-dimensional data.


.. [#Legacy] Legacy NJOY refers to `NJOY2016 <https://github.com/njoy/NJOY2016>`_
