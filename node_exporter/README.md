# Node_exporter for Prometheus
## Summary
This role installs and configures _node_exporter_ for Prometheus.

## Variables
Any variables in _defaults/main.yml_ can be redefined in a playbook. There are no mandatory variables, the role will work fine with default settings.

## Configuration of _node_exporter_
This role launches _node_exporter_ without any additional arguments except _-web.listen-address_. You may specify the listen address using _node_exporter_listen_address_ variable.

## systemd/sysvinit/upstart
This role is smart enough to choose init-system automatically. In case you would like to redefine it manually, use _node_exporter_service_mgr_ variable (possible values are _systemd_, _upstart_ or _sysvinit_).

## Dependencies
None.

In some cases, you should manually install _python2_ (absent in Ubuntu 16.04) and _libselinux-python_ (required for some versions of CentOS 6). Example playbook:
```
---
- hosts: all
  gather_facts: no
  pre_tasks:
    - name: install python2
      raw: test -e /usr/bin/python || (apt update && apt install -y python-minimal)
      changed_when: false
    - name: gather facts
      setup:
  tasks:
    - name: install libselinux-python binary for ansible to work
      yum: name=libselinux-python state=present
      when: "{{ ansible_pkg_mgr == 'yum' and not ansible_selinux }}"

- hosts: all
  roles:
    - role: node_exporter
```

## Platforms
All supported platforms are listed in _molecule.yml_.
