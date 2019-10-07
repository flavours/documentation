.. _reference:

Reference
##############

Notational Conventions
======================

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).

The key words "unspecified", "undefined", and "implementation-defined" are to be interpreted as described in the [rationale for the C99 standard](http://www.open-std.org/jtc1/sc22/wg14/www/C99RationaleV5.10.pdf#page=18).

An implementation is not compliant if it fails to satisfy one or more of the MUST, MUST NOT, REQUIRED, SHALL, or SHALL NOT requirements for the protocols it implements.
An implementation is compliant if it satisfies all the MUST, MUST NOT, REQUIRED, SHALL, and SHALL NOT requirements for the protocols it implements.




Addon managers
=================

A flavour addon manager (FAM) is responsible for all interactions with a corresponding platform. 


A platform is an opinionated project structure, framework and language. 
No clear definition of this exists and it must be defined by the platform developers.
Normally, we expect that this will boil down to only a hand full of platforms for each language/framework but there are no limits imposed upon any of this. 
As an example, multiple python + django platforms can exist, all with different project layouts. 
One platform will be an opinionated strucutre for the framework and addons can be installed into this structure. 
This can be very simple but there is no limit defined. 

A addon manager supports actions to `add` and `remove` addons of a platform into your project and can also `check` if an addon is compatible with a platform.  


The addon manager will do all the necessary file changes of the project to fully add the dependency. 
For example a "python" platform could just update the `requirements.txt` file during the `add` action of an addon.

Usage
-------

An addons manager MUST provide the following binaries, packaged in a docker image. 

.. code::
  
  /bin/add 
  /bin/remove
  /bin/check
  
All these binaries MUST accept the addon.yml file from stdin.  

add
++++

makes all required changes that is needed to install the addon for the platform. The actions are fully platform dependent.

* SHOULD add entry to app.flavour
* SHOULD 

* the hash function is sha256 hex



remove
+++++++

removes the addon, the actions are also fully platform dependent. 
The remove action is *not* necessarily the opposite of the `add` action, even though the name implies it.
The action can decide, for the sake of stability or security, to not undo certain changes which should normally be part of a full removal
The developers of this actions should keep the `remove` action as closely related to the `add` action as possible. 


check
+++++++

Returns `True` if its a valid file configuration file, `False` otherwise.


The container
-----------------

The addon manager must define its own name as an environment variable in the image.

.. code::

   FAM_IDENTIFIER='flavour/fam-python:0.1'


Conventions
------------

inheritance
+++++++++++

addon manager inheritance can be used to split generalized actions and tasks between addon managers. 


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

if is possible to "inherit" from other addon managers. this means, basically, basing you addon managers on top of existing ones. 
the fam-python addon manager (child) is based on the fam-flavour addon manager (parent) for example. 

as a convention, the child must move the standard binaries (/bin/bin, /bin/remove, /bin/check) to a separate subfolder with the name /bin/<fam-identifier> where the identifier is the name of the addon manager. e.g.: /bin/fam-flavour/add
the child binaries will be put in the normal place /bin/add and should internally invoke the parent binaries, if possible and desired. 


rego
++++

it is encouraged to verify the validity of a yaml file prior to acting on it. 
the action should fail as early as possible

rego can be used to validate yaml files early


error handling
+++++++++++++++++++

any error should fail the current process immediately with a non-zero exit code.
try to check as much as early as possible to fail as early as possible, if possible even before changing any files.




usage with the CLI
++++++++++++++++++++


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





.. _addons:


Addons
======

A flavour compatible addon is a normal addon or dependency of a platform (like python, php) which has additional information in form of a flavour addon configuration.
 
in the python world, this could be a normal pypi packge from pypi and other platforms and languages have other existing strucures which all can be expressed in the flavour addon configuration.

The addon has information about the compatibility and configuration options in a standadized way which can be used by different services.
This allowes automization and generation of configurations. 

Broadly speaking, there are two ways a flavour addon can be created. 

* Add the configuration directly into the native package for the platform
* Create a second package for the platform which configures the initial package. 
  This could be required if the first package does not want to support flavour or it is technically not feasible. 
  Using a second "configuration" package which mainly adds the flavour configuration would solve this issue.


It is very important to not that flavour does not intent to replace native package managers or ecosystems of platforms. 
The normal package managers and ecosystems are still used and flavour adds additional information and enables more functions due to the standadized approach. 



