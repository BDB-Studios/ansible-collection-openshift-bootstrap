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
rhcos_version: "4.13/latest"
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

httpd_image: "bdbstudios/httpd:el8-latest"
httpd:
  packages:
    present: []
    absent:
      - httpd
      - nginx
  ports:
    original: 80
    new: 8080
  directories:
    - /var/www/html/rhcos
  service: httpd
  home: /opt/httpd
  user:
    name: apache
    gid: 48
    uid: 48
    shell: /sbin/nologin
    home: /opt/httpd
  image_name: "{{ httpd_image }}"
```

Optional Variables

```yaml
# For Redhat
rhcos_version: "4.13/latest"
coreos:
  base_url: "https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/{{ rhcos_version }}/"
  files:
    - src: "rhcos-live-kernel-x86_64"
      dest: "/var/lib/tftpboot/rhcos/kernel"
    - src: "rhcos-live-initramfs.x86_64.img"
      dest: "/var/lib/tftpboot/rhcos/initramfs.img"
    - src: "rhcos-live-rootfs.x86_64.img"
      dest: "/var/www/html/rhcos/rootfs.img"
  install_disk: "/dev/{{ default_openshift_install_device }}"

```

```yaml
# For Fedora
fedcos_version: "38.20230609.3.0"
coreos:
  base_url: "https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/{{ fedcos_version }}/x86_64/fedora-coreos-{{ fedcos_version }}-"
  files:
    - src: "live-kernel-x86_64"
      dest: "/var/lib/tftpboot/rhcos/kernel"
    - src: "live-initramfs.x86_64.img"
      dest: "/var/lib/tftpboot/rhcos/initramfs.img"
    - src: "live-rootfs.x86_64.img"
      dest: "/var/www/html/rhcos/rootfs.img"
  install_disk: "/dev/{{ default_openshift_install_device }}"
```


Dependencies
------------

None

Example Playbook
----------------

```yaml
- name: Install and boot asset servers
  hosts: "bastion"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.boot_asset_servers
      tags:
        - always
...
```
License
-------

BSD
