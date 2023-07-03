bdbstudios.openshift_bootstrap.openshift_binaries
=========

Ansible role to install helm and openshift binary deps

Requirements
------------

None

Role Variables
--------------

```yaml
---

helm_version: "3.11.1"
openshift_version: "stable-4.13"

openshift:
  base_url: "https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/{{ openshift_version }}/"
  files:
    - file: "openshift-client-linux.tar.gz"
      creates: "/usr/bin/oc"
    - file: "openshift-install-linux.tar.gz"
      creates: "/usr/bin/openshift-install"

openshift_support_packages:
  - tar
  - zip
  - unzip
  - wget
  - curl

...

```

Dependencies
------------

None

Example Playbook
----------------

```yaml
---

- name: Ensure we have our openshift support binaries installed
  hosts: "bastion"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.openshift_binaries
      tags:
        - always
...
```

License
-------

BSD
