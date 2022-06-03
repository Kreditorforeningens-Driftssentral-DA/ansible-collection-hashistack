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
- hosts: all
  roles:
  - role: kred.hashistack_install
    vars:
      name: terraform
      version: '1.2.1'
  - role: kred.hashistack_install
    vars:
      name: consul
      version: '1.12.1'
      create_service: true
      config:
        datacenter: "dc1"
        data_dir: /var/opt/consul
        server: true
        bootstrap_expect: 1
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
