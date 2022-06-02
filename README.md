HASHISTACK-INSTALL
=========

Install HashiCorp using precompiled binaries.

Requirements
------------

N/A

Role Variables
--------------

| Variable | Default | Description | 
| :--      | :-:     | :--         |
| name | consul | Application to install |
| version | 1.12.1 | Version of the application to install |


Dependencies
------------

N/A

Example Playbook
----------------

```yaml
---
- hosts: all
  roles:
  - role: kred.hashistack_install
```

License
-------

MIT

Author Information
------------------

N/A
