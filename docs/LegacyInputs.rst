************
Legacy Input
************
The input for Legacy [#Legacy]_ NJOY begins with the case-insensitive name of the module that is to be run. Following the module name, various "cards" [#cards]_ are listed with each card having multiple arguments. Each card is terminated with an optional (but highly recommended) forward slash ``/`` followed by a carriage return. Any text following the forward slash is ignored and thus can be used as a comment. The input would look something like this: ::

   MODULE1
      arg1 arg2 arg3 / Card1
      arg4 arg5      / Card2
      ...
   MODULE2
      arg1 arg2      / Card1
   stop

The precise definitions of the arguments and cards for each module can be found in the `NJOY2016 manual <https://github.com/njoy/NJOY2016-manual/raw/master/njoy16.pdf>`_. Since NJOY21 uses the same input  as Legacy NJOY, we include some documentation here. 

General Information
===================
There are a few input arguments that are common among all/most modules that are worth explaining here.


.. glossary::
   input/output unit
      Most modules have arguments specifying the input and/or output "tapes". The argument is an integer, with a number between 20 and 99 inclusive. NJOY will read/write from/to actual files with the name ``tape##`` where ``##`` is the value of the input/output unit. These tape files must be in the same directory from which NJOY was called. Negative numbers refer to binary tapes and positive numbers refer to ASCII tapes.


.. [#Legacy] Legacy NJOY refers to the Fortran-based NJOY, namely, NJOY2016 and its predecessors.
.. [#cards] The term card refers to the physical `punch cards <https://en.wikipedia.org/wiki/Punched_card>`_ used in (very) old computer codes.
