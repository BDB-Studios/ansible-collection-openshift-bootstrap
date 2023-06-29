bdbstudios.openshift_bootstrap.bastion_config
=========

Updates SSHD Config.
Adds the required environment vars for OC to work correctly if enabled.
Adds autocomplete for oc and kubectl.
Cleans up MOTD cockpit nag

Requirements
------------

None

Role Variables
--------------

```yaml
sshd:
  permit_root_login: "no"
  allow_agent_forwarding: "no"
  allow_tcp_forwarding: "yes"
  extra_accept_env:
    - "KUBECONFIG"

bastion_packages:
  pip:
    - kubernetes>=12.0.0
    - PyYAML >= 3.11
    - jsonpatch
    - jmespath

define_oc_env: false

```

Dependencies
------------

None

Example Playbook
----------------

```yaml
---

- name: Configure our bastion host
  hosts: "bastion"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.bastion_config
      vars:
        - define_oc_env: true
      tags:
        - always
...
```

License
-------

GPL-2.0-or-later
