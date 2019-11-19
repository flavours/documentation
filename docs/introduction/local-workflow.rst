.. _local-workflow:

Local workflow
#################

Your first steps with the Flavours CLI and the Flavours registry. This example uses PHP and Laravel; the principles
for other stacks are the same.


Set up your enivironment
==========================================

Two components are required, Docker and the Flavours CLI.

Install:

Please install:

* Docker for Mac, Docker for Windows or Docker CE for your Linux
* the Flavours CLI, from https://www.npmjs.com/package/@flavour/cli; shortcut: ``curl -L
  https://flavours.dev/cli/install.sh | sh``


Create a project
===========================

Clone the "getting-started-with" project for the PHP stack::

   git clone https://github.com/flavours/getting-started-with-php
   cd getting-started-with-php


Install an addon into the project
=================================

We will add the `laravel-responsecache <https://www.laravelplay.com/packages/spatie::laravel-responsecache>`_ package.
It's an open-source addon, released the Belgian agency `Spatie <https://spatie.be/opensource>`_, and can improve
performance of Laravel sites by caching responses.

In your project directory, run::

   flavour add composer/spatie/laravel-responsecache:6.1.1`

The ``flavour`` CLI will check the default registry at addons.flavours.dev and look for this addon.

You can take a look at the :ref:`background` section of this documentation for more information.

This is all you have to do to trigger a simple installation. Use ``git diff`` to see what changes have been applied to
the project.
