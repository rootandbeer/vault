---
categories:
  - "[[Guides]]"
topics:
  - "[[Proxmox]]"
  - "[[Python]]"
created:
---


Proxmoxer is required for creating init template

```shell
# create a new virtual environment folder
python3 -m venv ~/ansible-venv

# activate it
source ~/ansible-venv/bin/activate

# upgrade pip inside venv (optional but recommended)
pip install --upgrade pip

# install proxmoxer inside venv
pip install proxmoxer requests ansible

ansible-galaxy collection install community.proxmox --upgrade

# run ansible-playbook from the same shell/session
ansible-playbook create_cloud_init_template.yml --ask-vault-pass

```