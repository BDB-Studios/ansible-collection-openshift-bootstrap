bdbstudios.openshift_bootstrap.dhcpd_server
=========

Installs the Bind DNS server on the target bastion machine.

Creates forward and reverse domain entries for the openshift domain. Will create entries for bastion, workers, controls_planes,
bootstrap and optionally the nfs_server.

Requirements
------------

Ansible galaxy collections `containers.podman` and `ansible.posix`

Role Variables
--------------

```yaml
named_image: "bdbstudios/named:el8-latest"

bind:
  home: /opt/named
  service: named
  image_name: "{{ named_image }}"
  packages:
    present:
      - bind-utils
    absent:
      - bind
  user:
    name: named
    uid: 25
    gid: 25
    home: "/opt/named"
    shell: "/bin/false"

```

Optional vars, will log us into podman/docker registries
```yaml
podman_registries:
  - registry: docker.io
    username: foo
    password: bar
```

External vars to generate our dhcpd config on a flat network
```yaml

bastion:
  ipaddr: 192.168.0.10
  networkifacename: eth0

bootstrap:
  - name: "bootstrap"
    ipaddr: "192.168.0.11"
    macaddr: "52:54:00:a0:e5:2a"
control_planes:
  - name: "controlplane01"
    ipaddr: "192.168.0.21"
    macaddr: "52:54:00:59:8b:b8"

  - name: "controlplane02"
    ipaddr: "192.168.0.22"
    macaddr: "52:54:00:46:06:22"

  - name: "controlplane03"
    ipaddr: "192.168.0.23"
    macaddr: "52:54:00:47:fd:19"
```

Vlan config
```yaml

vlan_config: &VLAN
  id: 200
  name: "vlan200"
  router: # Vlan Router IP
  subnet_mask: # Vlan Netmask
  forwarder: # Vlan Forwader
  broadcast: # Vlan Broadcast

bootstrap: &BOOTSTRAP
  - name: "bootstrap"
    ipaddr: "192.168.0.11"
    macaddr: "52:54:00:a0:e5:2a"
    vlan: *VLAN
control_planes: &CONTROL_PLANES
  - name: "controlplane01"
    ipaddr: "192.168.0.21"
    macaddr: "52:54:00:59:8b:b8"
    vlan: *VLAN
  - name: "controlplane02"
    ipaddr: "192.168.0.22"
    macaddr: "52:54:00:46:06:22"
    vlan: *VLAN
  - name: "controlplane03"
    ipaddr: "192.168.0.23"
    macaddr: "52:54:00:47:fd:19"
    vlan: *VLAN

vlans:
  - <<: *VLAN
    network: # Vlan Network
    prefix: # Vlan Network Prefix
    networkifacename: # Interface listening to vlan traffice
    ipaddr: # IP address of next server on vlan
    bootstrap: *BOOTSTRAP
    control_planes: *CONTROL_PLANES
    workers: []

```

Required for both vlans and flat network
```yaml
# For our routing and next servers
networking_defaults:
  gateway: 192.168.0.1
  broadcast: 192.168.0.254
  subnet_mask: 255.255.255.0
  ntp:
    - 8.8.8.8
    - 1.1.1.1

dns:
  forwarder1: 192.168.0.1
  forwarder2: 8.8.8.8
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

- name: Install and our dhcpd server
  hosts: "all"
  become: true
  roles:
    - role: bdbstudios.openshift_bootstrap.dhcpd_server
      tags:
        - always
...
```

License
-------

GPL-2.0-or-later
