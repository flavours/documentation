.. _reference:

Reference
##############

Technical reference - tools, components and commands


Architectural overview
======================

flavour can manage the configuration and dependency of addons in a project.
An addon is loosely defined as anything that can be added to a project of any stack.
A stack is also very loosely defined and can be viewed as a combination of a programming language, framework and best practices. 
This also means that there can be multiple `stacks` for a python - django project but each with different project layouts and other practices.
This open nature will ensure that any project, regardless of its kind and requirements can be used as a stack and new and small stacks are first class citizens in the eco system.

A `flavour addon manager(fam)` is used to manage a stack in the flavour context. 
Each addon manager implements a fixed interface - see below - which allows a unified usage and interchangeability.


Addons
======

A flavour compatible addon is a normal addon or dependency of a stack (like python, php) which has additional information in form of a flavour addon configuration.
 
in the python world, this could be a normal pypi package from pypi and other stacks and languages have other existing structures which all can be expressed in the flavour addon configuration.

The addon has information about the compatibility and configuration options in a standardized way which can be used by different services.
This allows automation and generation of configurations. 

Broadly speaking, there are two ways a flavour addon can be created. 

* Add the configuration directly into the native package for the stack
* Create a second package for the stack which configures the initial package. 
  This could be required if the first package does not want to support flavour or it is technically not feasible. 
  Using a second "configuration" package which mainly adds the flavour configuration would solve this issue.


It is very important to not that flavour does not intent to replace native package managers or ecosystems of stacks. 
The normal package managers and ecosystems are still used and flavour adds additional information and enables more functions due to the standardized approach. 




Addon managers
=================

A flavour addon manager (FAM) is responsible for all interactions with a corresponding stack. 

A stack is an opinionated project structure, framework and language. 
No clear definition of this exists and it must be defined by the stack developers.
Normally, we expect that this will boil down to only a hand full of stacks for each language/framework but there are no limits imposed upon any of this. 
As an example, multiple python + django stacks can exist, all with different project layouts. 
One stack will be an opinionated structure for the framework and addons can be installed into this structure. 
This can be very simple but there is no limit defined. 

A addon manager supports actions to `add` and `remove` addons of a stack into your project and can also `check` if an addon is compatible with a stack.  


The addon manager will do all the necessary file changes of the project to fully add the dependency. 
For example a "python" stack could just update the `requirements.txt` file during the `add` action of an addon.

Actions
-------

An addons manager MUST provide the following binaries, packaged in a docker image. 

.. code::
  
  /bin/add 
  /bin/remove
  /bin/check
  
All these binaries MUST accept the addon.flavour file from stdin.  

add
++++

Makes all required changes that is needed to install the addon for the stack. The actions are fully stack dependent.

* SHOULD add a new entry to the "addons" section in the app.flavour
* The hash function MUST be sha256 hex



remove
+++++++

Removes the addon, the actions are also fully stack dependent. 
The remove action is *not* necessarily the opposite of the `add` action, even though the name implies it.
The action can decide, for the sake of stability or security, to not undo certain changes which should normally be part of a full removal
The developers of this actions should keep the `remove` action as closely related to the `add` action as possible. 


check
+++++++

Returns `True` if its a valid `addon.flavour` file, `False` otherwise.


Configuration
-----------------

The addon manager must define its own name as an environment variable in the image.

.. code::

   FAM_IDENTIFIER='flavour/fam-python:0.1'




app.flavour and addon.flavour yaml files
========================================

You can find examples online in the python reference implementation of the spec at https://github.com/flavours/libflavour/tree/master/libflavour/test/data.

The following is an example addon for the `aldryn`-stack. 

.. code-block:: yaml

   
   spec: 0.1
   meta:
     name: django-divio
     version: 0.1
   install: 
     package: django==1.11.20.4


.. glossary::

    spec
       Specifies the version of the flavour specification. Required

    meta
       General information about the addon / project like `name` or `version`. Both fields are required.

    install
       Key-Value structure which is used during the addon manager actions (e.g. `add`, `remove`).
       This is purely defined and unique to each stack and will change for each stack.
       In this case, the `aldryn` stack requires a `package` key which has a native python package as a value.
       



This is an example of an `app.flavour` file which could be found in a project which supports the `aldryn`-stack. : 

.. code-block:: yaml

   spec: 0.1
   meta:
     name: my-aldryn-project
     version: 0.1
   addons:
     addon/aldryn-addons:1.0.4:
       manager: flavour/fam-aldryn:0.1
       hash: 1cf06ba56949fe7370d81b9ba459a272cf1879036d9a363a119cd441d8854182
     addon/aldryn-common:1.0.4:
       manager: flavour/fam-aldryn:0.1
       hash: f2c5818177ea75546d2e18d65f2d6890ddfa7d87fc617d7200c9df7c2f9857f2

The `spec`and `meta` are the same for addons and projects.

.. glossary::

    addons
       A list of installed addons. 

       .. code-block:: yaml
    
          # Name of the addon and version 
          addon/aldryn-common:1.0.4:
             # Name of the addon manager that was used during installation and version
             manager: flavour/fam-aldryn:0.1 
             # sha256 hex of the configuration content of the addon that was used during installation
             hash: f2c5818177ea75546d2e18d65f2d6890ddfa7d87fc617d7200c9df7c2f9857f2 



CLI
===

The command line interface is the main mode of interaction with flavour during normal development. 
It exposes the basic functionality of flavour addon managers and allows for installation, removal and configuration checks.

https://github.com/flavours/cli
