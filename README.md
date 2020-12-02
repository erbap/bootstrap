bootstrap
=========

This Ansible role performs som initial configuration:
- create the host_vars/<inventory_hostname> directory
- create/maintain a bootstrap.yml file in the host_vars/<inventory_hostname> directory
- basic (raw) installation from essential packages
- configure the FQDN hostname

**Note:** This role is still in active development. There may be unidentified issues and the role variables may change as development continues.

This role is developed wit ansible version 2.9.7


## Role Variables

### `ansible_managed:`

I consider it good practice to define this variable in your ansible.cfg and add this to all template files (where possible).

### `ansible_python_interpreter:`

[delegate_to uses incorrect interpreter](https://github.com/ansible/ansible/issues/63180)

[Ansible Interpreter Discovery for the SEP server(s)](https://docs.ansible.com/ansible/latest/reference_appendices/interpreter_discovery.html)

```
[WARNING]: Platform linux on host <inventory_hostname> is using the discovered Python interpreter at <some_python_interpreter>, but future installation of another Python interpreter could change this.`
```

To stop this warning and prevent problems with delegate_to I define this variable during the bootstrap.

### `bootstrap_fqdn:`

The FQDN from the host. By default the hostname is set to the machine name during installation from the OS, without a domain suffix.

You may test this on a new installed host by `ansible -m setup <inventory_hostname> | grep 'ansible_hostname\|ansible_domain\|ansible_fqdn'`.

By default I take the <inventory_hostname> as FQDN name but this is easly overwritten from the command line (see later).

A lot of software depend on a proper configured FQDN name that is resolved by a propper configured DNS server.


## example playbook

vi play_bootstrap.yml
```
---
- hosts: all
  become: True

  roles:
  - { role: erbap.bootstrap }
...
```

You can set the FQDN name from the command line:

```
ansible-playbook play_bootstrap.yml --limit inventory_hostname --extra-vars 'bootstrap_fqdn=host.domain.tld'
```


## License

BSD-2-Clause-Patent

Like MIT and BSD-2-Clause and unlike Apache License v2, BSD+Patent is compatible with GPLv2.

Like Apache License v2 and unlike MIT and BSD-2-Clause, BSD+Patent avoids leading to potential patent trolls.
