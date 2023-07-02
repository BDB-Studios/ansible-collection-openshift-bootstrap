bdbstudios.openshift_bootstrap.squid_proxy
=========

Provides a squid proxy server for the openshift nodes to connect to the internet through. Ultimately we
do not want the openshift workers/control planes to have direct internet access so this provides them a mechanism
to pull required binaries/images via the proxy on the bastion.

Requirements
------------

Ansible galaxy collections `containers.podman` and `ansible.posix`

Role Variables
--------------

```yaml
squid_image: "bdbstudios/squid:el8-latest"

proxy:
  packages:
    present:
      - "httpd-tools"
    absent:
      - squid
  pip_packages:
    - passlib
  service: squid
  port: "{{ proxy_port | default('3128') }}"
  credentials:
    user: "{{ proxy_user | default('coreos') }}"
    password: "{{ proxy_password | default('coreospassword') }}"
  home: "/opt/squid"
  user:
    name: squid
    uid: 23
    gid: 23
    home: "/opt/squid"
    shell: "/sbin/nologin"
  image_name: "{{ squid_image }}"


```

Required for so we can configure our proxy url
```yaml
# For our routing and next servers

dns:
  clusterid: test
  domain: openshift.local

```
Dependencies
------------

None

Example Playbook
----------------

```yaml
---

- name: Install and our squid proxy server
  hosts: "all"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.squid_proxy
      tags:
        - always
...
```

License
-------

GPL-2.0-or-later
