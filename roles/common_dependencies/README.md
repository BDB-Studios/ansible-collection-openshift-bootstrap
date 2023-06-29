bdbstudios.openshift_bootstrap.common_dependencies
=========

Install a basic set of commong dependencies

Requirements
------------

Ansible galaxy collection `containers.podman`

Role Variables
--------------

```yaml
dependencies:
  packages:
    - podman
    - podman-compose
    - podman-docker
    - jq
    - python3-jmespath
    - bash-completion
  repos:
    - "https://dl.fedoraproject.org/pub/epel/epel-release-latest-{{ ansible_distribution_major_version }}.noarch.rpm"

```

Optional vars, will log us into podman registries
```yaml
podman_registries:
  - registry: quay.io
    username: foo
    password: securepassword
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
---

- name: Install and configure dependencies keys
  hosts: "all"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.common_dependencies
      tags:
        - always
...
```

License
-------

GPL-2.0-or-later
