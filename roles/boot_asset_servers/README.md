bdbstudios.openshift_bootstrap.boot_asset_servers
=========
Creates our ignition files

Creates an apache http server that listens on port 8080. This server contains the ignition files as well as the initial linux
boot image required for the first portion of the PXE boot. Alternately we can use it to stage a
CoreOS iso as well as a boot asset if we have to manually configure machines.

Also configures a tftp server to distribute core linux files during the PXE boot process

Requirements
------------

None

Role Variables
--------------

Dependencies
------------

None

Example Playbook
----------------


License
-------

BSD
