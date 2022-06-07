# HASHISTACK-INSTALL
Install HashiCorp using precompiled binaries.

## Requirements
N/A

## Role Variables
| Variable | Default | Description | 
| :--      | :-:     | :--         |
| app_name | consul | Application to install |
| app_version | 1.12.1 | Version of the application to install |


## Dependencies
N/A

## Example Playbook
See https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#using-roles

```yaml
# playbook.yml
---
- hosts: localhost
  connection: local
  gather_facts: true
  tasks:
  - ansible.builtin.include_role:
      name: kred.hashistack_install
    vars:
      app_name: "{{ item.app }}"
      app_version: "{{ item.version }}"
      manage_service: "{{ item.create_service | default(false,false) }}"
      manage_config: "{{ item.config | default({},false) }}"
    loop:
    - app_name: terraform
      app_version: '1.2.2'
    - app_name: nomad
      app_version: '1.3.1'
    - app_name: consul
      app_version: '1.12.1'
```

```bash
# Download/install role
ansible-galaxy role install <role>

# Run playbook
ansible-playbook -i <hostsfile> <playbook>
```

## License
MIT

## Author Information
N/A
