.. _background:


Background
###########

Explanation of design decisions. 

using STDIN in the addon managers
======================================

The `action` binaries in an addon manager only accept a single YML file from STDIN. 
No other parameters should be needed.


addon manager inheritance
==========================

Addon manager inheritance is a core feature and is used to simplify the required logic for each platform. 
It is a recommendation to place the 3 required binaries (`add`, `remove` and `check`) in a subfolder under `bin` and create symlinks to these binaries.
For example, the `fam-php-laravel` should place its binaries under `/bin/fam-php-laravel/add` and create a symlink in `/bin/add` to the final location.
This allows for a flexible structure for the different binaries during a complex inheritance architecture while also providing well defined locations for all binaries .
