# Blackbox_exporter for Prometheus
## Summary
This role installs and configures _blackbox_exporter_ for Prometheus.

## Variables
Any variables in _defaults/main.yml_ can be redefined in a playbook. There are no mandatory variables, the role will work fine with default settings.

## Configuration of _blackbox_exporter_
If you would like to overwrite the default configuration, specify a path to custom configuration file in _blackbox_config_template_path_ variable.

## systemd/sysvinit/upstart
This role is smart enough to choose init-system automatically. In case you would like to redefine it manually, use _blackbox_service_mgr_ variable (possible values are _systemd_, _upstart_ or _sysvinit_).

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
    - role: blackbox_exporter
```

## Platforms
All supported platforms are listed in _molecule.yml_.
