************
Legacy Input
************
The input for Legacy [#Legacy]_ NJOY begins with the case-insensitive name of the module that is to be run. Following the module name, various "cards" [#cards]_ are listed with each card having multiple arguments. Each card is terminated with an optional highly recommended) forward slash ``/`` followed by a carriage return. Any text following the forward slash is ignored and thus can be used as a comment. 

.. tip::
   It is highly recommended that you terminate every Card with a forward slash---even though it is not required. This forces NJOY to stop looking for more input for that Card. It also helps the writer of the input to know where things end.


The last line of the input is the word ``stop``, which tells NJOY to stop processing. The input would look something like this:

.. code-block:: txt

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
   comments
      Lines that begin with two hyphens followed by a space, ``--``---and that occur *before* the module name---are ignored by the input parsers. These lines can be used as comments. Also, everything after the forward slash terminating a Card is ignored and can also be treated as a comment.

   input/output unit
      Most modules have arguments specifying the input and/or output "tapes". The argument is an integer, with a number between 20 and 99 inclusive. NJOY will read/write from/to actual files with the name ``tape##`` where ``##`` is the value of the input/output unit. These tape files must be in the same directory from which NJOY was called. Negative numbers refer to binary tapes and positive numbers refer to ASCII tapes.


.. [#Legacy] Legacy NJOY refers to the Fortran-based NJOY, namely, NJOY2016 and its predecessors.
.. [#cards] The term card refers to the physical `punch cards <https://en.wikipedia.org/wiki/Punched_card>`_ used in (very) old computer codes.
