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

Actions
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


Configuration
-----------------

The addon manager must define its own name as an environment variable in the image.

.. code::

   FAM_IDENTIFIER='flavour/fam-python:0.1'


Usage with the CLI
-------------------

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



