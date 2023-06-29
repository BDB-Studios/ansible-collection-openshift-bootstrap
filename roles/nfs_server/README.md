bdbstudios.openshift_bootstrap.nfs_server
=========

Creates a NFS server to be used by the registry and other applications as persistent storage.

 - If we have 2 or more  JBOD disks will create a striped LVM set to persist data
 - If we have 1 JBOD disk this will create a LVM that can later be expanded
 - If we have no JBODs defined this will create a share on the primary disk

Requirements
------------

Requires `ansible.posix` collection to be installed

Role Variables
--------------

Global variables
```yaml
nfs:
  base_dir: /data/nfs # The path for our nfs directory
  packages: # Packages required
    - lvm2
    - parted
    - nfs-utils
  export_directories: # What directories to create and export
    - registry
    - applications
    - storage

```

Disk config variables

```yaml
mount_point: /opt # Where we are going to disks
storage_disks: # VG disk dictionary
  - vg_name: "registry.volgroup"
    volume_name: registry_data
    lvm_disk_size: "1.8T"
    lvm_disks:
      - sdb
      - sdc
      - sdd
      - sde
      - sdf
      - sdg
      - sdh
      - sdi
      - sdj
      - sdk
      - sdl
      - sdm
```

Example Playbook
----------------


```yaml
- name: Configure NFS Server
  hosts: nfs_server
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.nfs_server
  tags:
    - nfs
```

License
-------

MIT
