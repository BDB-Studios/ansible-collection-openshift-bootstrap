bdbstudios.openshift_bootstrap.users
=========

Helper to manage users

Requirements
------------

None

Role Variables
--------------

```yaml

user_list:
  - name: adminuser
    system: false
    password: "{{ admin_password }}"
    sudoers:
      commands:
        - ALL
      state: present
      nopassword: true
    append_groups: true
    groups:
      - wheel
  - name: quay
    system: true
  - name: packer
    state: absent

```

Dependencies
------------

Requires the `ansible.posix` collection for ssh key auth management and the `community.general` for sudoers

Example Playbook
----------------

```yaml
  name: Manage Users
  hosts: all
  become: true

  roles:
    - role: bdbstudios.openshift_bootstrap.users
      tags:
        - always

```
License
-------

GPL-2.0-or-later
