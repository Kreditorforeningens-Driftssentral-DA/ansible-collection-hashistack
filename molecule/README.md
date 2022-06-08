# ansible-role-hashistack-install
Installation of HashiCorp applications

## Ansible
* [Contributing](https://galaxy.ansible.com/docs/contributing/index.html)


## Molecule
  * [Creating role](https://molecule.readthedocs.io/en/latest/getting-started.html#creating-a-new-role)
  * [Developing & testing using Molecule](https://www.ansible.com/blog/developing-and-testing-ansible-roles-with-molecule-and-podman-part-1)

```bash
# Requirements
ansible-galaxy collection install -v community.docker:>=1.9.1
```

## Setup local molecule environment

```bash
export TARGET_VENV=/opt/venv/molecule

# Install required os-packages
apt-get -qqy install --no-install-recommends python3-minimal python3-pip python3-venv openssh-client
python3 -m venv ${TARGET_VENV}

# Configure molecule environment
. ${TARGET_VENV}/bin/activate
python3 -m pip install --upgrade pip wheel
python3 -m pip install --upgrade ansible-core ansible-lint
python3 -m pip install --upgrade paramiko lxml molecule molecule-docker

# Install ansible collections
ansible-galaxy collection install ansible.posix
ansible-galaxy collection install community.general
ansible-galaxy collection install community.docker
ansible-galaxy collection install community.vagrant
molecule --version

# Run default scenario
molecule converge
```

