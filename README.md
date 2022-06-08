# HASHISTACK-INSTALL

Install, configure & create service definition for HashiCorp applications. Each option is optional & can be managed seperately.

# Support matrix

| Name            | Binary | Config | Service | Description | 
| :--             | :-:    | :-:    | :-:     | :--         |
| Boundary        | X | X | X | https://www.boundaryproject.io/ |
| Consul          | X | X | X | https://www.consul.io/ |
| Consul-Template | X | - | - | https://github.com/hashicorp/consul-template |
| Nomad           | X | X | X | https://www.nomadproject.io/ |
| Packer          | X | - | - | https://www.packer.io/ |
| Terraform       | X | - | - | https://www.terraform.io/ |
| Vagrant         | - | - | - | https://www.vagrantup.com/ |
| Vault           | X | X | X | https://www.vaultproject.io/ |
| Waypoint        | X | X | X | https://www.waypointproject.io/ |


## Requirements

Optional installation & configuration of CNI-plugins require the following ansible collections:

```bash
# cni_install: true
ansible-galaxy collection install community.general # modprobe
ansible-galaxy collection install ansible.posix # sysctl
```

## Role Variables

| Variable | Default | Description | 
| :--      | :-:     | :--         |
| manage_binary | true | Install binary |
| manage_config | false | Create configuration files & folders |
| manage_service | false | Create service-definition |
| app_name | terraform | Application to install |
| app_version | '1.2.2' | Application version to install |
| app_force_install | false | Force re-installation (ignore current version) |
| app_config | {} | YAML-formatted configuration to render as json-file |
| cni_install | false | Install CNI-plugins (consul-connect & nomad bridge-network) |
| cni_version | '1.1.1' | CNI-plugins version |


## Dependencies
N/A

## Example Playbook
See https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#using-roles

```yaml
# playbook.yml
# > Install Terraform & Nomad (incl. service)
# > Add 2 separate configuration-files for Nomad
---
- name: Converge
  hosts: all
  gather_facts: true

  # play-level roles
  roles: []
 
  tasks:
  - name: Install Terraform, Packer & Nomad
    include_role:
      name: ansible-role-hashistack-install
    vars:
      app_name: "{{ item.name }}"
      app_version: "{{ item.version }}"
    loop:
    - name: terraform
      version: '1.2.2'
    - name: packer
      version: '1.8.1'
    - name: nomad
      version: '1.3.1'
  
  - name: Configure Nomad (multiple config-files)
    include_role:
      name: ansible-role-hashistack-install
    vars:
      app_name: nomad
      manage_binary: false
      manage_service: true
      manage_config: true
      manage_config_file: "{{ item.file }}"
      app_config: "{{ item.content }}"
    loop:
    - file: server
      content:
        datacenter: dc1
        data_dir: /var/opt/nomad
        server:
          enabled: true
          bootstrap_expect: 1
    - file: client
      content:
        client:
          enabled: false
        plugin:
          raw_exec:
            config:
              enabled: true
    
  - name: Configure Consul (skip installation)
    include_role:
      name: ansible-role-hashistack-install
    vars:
      app_name: consul
      manage_binary: false
      manage_config: true
      app_config:
        datacenter: dc1
        data_dir: /var/opt/consul
        server: true
        bootstrap_expect: 1
        client_addr: '0.0.0.0'
        ui_config:
          enabled: true
        performance:
          raft_multiplier: 8
        ports:
          grpc: 8502
        connect:
          enabled: true
  
  - ansible.builtin.shell:
      cmd: nohup /usr/local/bin/nomad agent -config /etc/opt/nomad.d &
    when: false
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
