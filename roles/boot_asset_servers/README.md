bdbstudios.openshift_bootstrap.boot_asset_servers
=========
Creates our ignition files

Creates an apache http server that listens on port 8080. This server contains the ignition files as well as the initial linux
boot image required for the first portion of the PXE boot. Alternately we can use it to stage a
CoreOS iso as well as a boot asset if we have to manually configure machines.

Also configures a tftp server to distribute core linux files during the PXE boot process

Requirements
------------

Ansible galaxy `ansible.posix` and `containers.podman` collections

Role Variables
--------------

```yaml

pull_secret: '{"auths":{"none":{"auth": "none"}}}'

tfptd_image: "bdbstudios/tftpd:el8-latest"
tftpd:
  packages:
    absent:
      - tftp-server
      - syslinux
    present:
      - dnf-utils
  service: "tftp"
  home: /opt/tftp
  user:
    name: ftp
    uid: 14
    gid: 50
    home: /opt/tftp
    shell: /sbin/nologin
  image_name: "{{ tfptd_image }}"

```
Dependencies
------------

None

Example Playbook
----------------


License
-------

BSD
