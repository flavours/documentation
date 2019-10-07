.. _background:


Background
###########

Addon Managers
=================


Explanation of design decisions. 

using STDIN in the addon managers
---------------------------------

The `action` binaries in an addon manager only accept a single YML file from STDIN. 
No other parameters should be needed.


inheritance
-----------

Addon manager inheritance is a core feature and is used to simplify the required logic for each platform. 

.. graphviz::

   digraph pipeline {
      node[shape=box];
      subgraph cluster0 {
            color=lightgrey;
            label="Addon Managers";
            flavour->python;
            flavour->divio_legacy_addons
            flavour->bash;
            flavour->php;
            php->laravel;
            php->symfony;
      }
   }

It is a recommendation to place the 3 required binaries (`add`, `remove` and `check`) in a subfolder under `bin` and create symlinks to these binaries.
For example, the `fam-php-laravel` should place its binaries under `/bin/fam-php-laravel/add` and create a symlink in `/bin/add` to the final location.
This allows for a flexible structure for the different binaries during a complex inheritance architecture while also providing well defined locations for all binaries .


rego and checks
---------------

It is encouraged to verify the validity of a YAML file prior to acting on it and the action should fail as early as possible.
`rego` can be used to validate YAML files early and you can already find examples of this in existing addon managers.


General
=======

errors
-------

Any error should fail the current process immediately with a non-zero exit code.
Try to check as much as early as possible to fail as early as possible, if possible even before changing any files.
Make use of the `check` action for this.

