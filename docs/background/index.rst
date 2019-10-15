.. _background:


Background
###########


General
=======

errors
-------

Any error should fail the current process immediately with a non-zero exit code.
Try to check as much as early as possible to fail as early as possible, if possible even before changing any files.
Make use of the `check` action for this as it SHOULD get executed before the `add` and `remove` actions.


Addon Managers
=================

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

Example of the action binaries and symlinks of an addon manager called `fam-aldryn` which inherits from `fam-flavour`:


.. code::
  
  /bin/fam-flavour/add
  /bin/fam-flavour/remove
  /bin/fam-flavour/check
  /bin/fam-aldryn/add
  /bin/fam-aldryn/remove
  /bin/fam-aldryn/check
  /bin/add -> /bin/fam-aldryn/add
  /bin/remove -> /bin/fam-aldryn/remove
  /bin/check -> /bin/fam-aldryn/check

Checking YAML files - rego
---------------------------

It is encouraged to verify the validity of a YAML file prior to acting on it and fail in case the check does not succeed.
`rego` can be used to validate YAML files early and you can already find examples of this in existing addon managers like `fam-flavour`.




CLI
===



internals: installation highlevel overview
------------------------------------------

This is a high level sequence diagram of all the actors involved during installation of an addon.

.. mermaidjs::
   

   sequenceDiagram
       participant user
       participant flavour cli
       participant hub flavour
       participant flavour addon manager(fam)
       participant project

       user->>flavour cli:flavour add divio/django
       flavour cli->>hub flavour:resolve divio/django
       hub flavour-->>flavour cli: data:addon_id
       flavour cli->>hub flavour: get detail of addonversion by addon_id
       hub flavour-->>flavour cli: data
       flavour cli->>flavour cli: figure out platform of addonversion
       flavour cli->>hub flavour: get detail of platform
       hub flavour-->>flavour cli: data
       flavour cli->>flavour cli: select fam of the platform
       flavour cli->>flavour addon manager(fam): add addon based on yaml
       flavour addon manager(fam)->>project: change files to install the addon in the code
       project-->>flavour addon manager(fam):success
       flavour addon manager(fam)-->>flavour cli:success
       flavour cli-->>user:success




