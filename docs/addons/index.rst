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



