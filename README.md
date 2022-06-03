# HASHISTACK-INSTALL

Install HashiCorp using precompiled binaries.

## Requirements

N/A

## Role Variables

| Variable | Default | Description | 
| :--      | :-:     | :--         |
| name | consul | Application to install |
| version | 1.12.1 | Version of the application to install |


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
      app: "{{ item.app }}"
      version: "{{ item.version }}"
      create_service: "{{ item.create_service | default(false,false) }}"
      config: "{{ item.config | default({},false) }}"
    loop:
    - app: terraform
      version: '1.2.2'
    - app: nomad
      version: '1.3.1'
    - app: consul
      version: '1.12.1'
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
