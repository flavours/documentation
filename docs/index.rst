.. raw:: html

    <style>
        .row {clear: both}

        .column img {border: 1px solid black;}

        @media only screen and (min-width: 1000px),
               only screen and (min-width: 600px) and (max-width: 768px){
            .column {
                padding-left: 5px;
                padding-right: 5px;
                float: left;
                width: 25%;
            }
        }
        h2 {border-top: 1px solid black; padding-top: 1em}
    </style>


==============================================
Flavour v0.1 - Documentation and Specification
==============================================

.. image:: _static/flavours_512.png
   :alt: 'Flavours'
   :class: 'main-visual'


Contents
========

.. rst-class:: clearfix row

.. rst-class:: column


:ref:`Introduction <introduction>`
----------------------------------

Get started


.. rst-class:: column

:ref:`Addon Managers <addonmanagers>`
-------------------------------------

What are platforms and how do they work

.. rst-class:: column

:ref:`Addons <addons>`
----------------------------

Advantages or flavour addons and ways to expose functionallity as a addon developer.


.. rst-class:: column


:ref:`Specification <specification>`
------------------------------------

The specification.

.. rst-class:: clearfix row



About this documentation
=========================

This documentation is the central hub of information for all things flavour.



Detailed table of contents
===========================

.. toctree::
   :maxdepth: 2

   introduction/index
   addonmanagers/index
   addons/index
   specification/index



Notational Conventions
----------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).

The key words "unspecified", "undefined", and "implementation-defined" are to be interpreted as described in the [rationale for the C99 standard](http://www.open-std.org/jtc1/sc22/wg14/www/C99RationaleV5.10.pdf#page=18).

An implementation is not compliant if it fails to satisfy one or more of the MUST, MUST NOT, REQUIRED, SHALL, or SHALL NOT requirements for the protocols it implements.
An implementation is compliant if it satisfies all the MUST, MUST NOT, REQUIRED, SHALL, and SHALL NOT requirements for the protocols it implements.




Indices and tables
----------------------

* :ref:`genindex`
* :ref:`search`
