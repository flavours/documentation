.. _howtoguides:



How-to guides
#################


How-to: Setup the flavour locally
==========================================

To use flavour locally, you just have to install `docker` and get the flavour CLI.

You can get the flavour CLI from https://www.npmjs.com/package/@flavour/cli or just use the following command ::

   curl -L https://flavours.dev/cli/install.sh | sh


How-to: Get your own addon registry
===================================

We currently offer a proof-of-concept registry for you to try. Please check out `https://github.com/flavours/registry-proof-of-concept` for further instructions on how to set it up.


How-to: Install a new addon
===========================

In this example, we will add a new addon / dependency to a project.
We will use PHP for this example but the general steps are transferable to other platforms as well.

Download the "getting-started-with" project for the PHP-platform ::

   git clone https://github.com/flavours/getting-started-with-php
   cd getting-started-with-php

Install a new addon. In this case, we use the pre-prepared `laravel-responsecache` in version `6.1.1`. ::

   flavour add composer/spatie/laravel-responsecache:6.1.1`

The flavour CLI will check the default registry on addons.flavours.dev and look for this addon. 
You can take a look at the background section of this documentation to get more details.
This is all you have to do to trigger a simple installation. You can check out the changes in the project now. 
