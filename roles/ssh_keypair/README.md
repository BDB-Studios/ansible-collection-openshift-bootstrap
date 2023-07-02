bdbstudios.openshift_bootstrap.ssh_keypair
=========

Ansible role to generate a ssh keypair for Openshift CoreOS.

Requirements
------------

Requires `community.crypto` ansible collection

Role Variables
--------------

```yaml
# Optional vars
coreos_ssh_public_key: ~
coreos_ssh_key: ~
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
---

- name: Ensure we have our ssh keys for coreos
  hosts: "bastion"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.squid_keypair
      tags:
        - always
...
```

License
-------

BSD
