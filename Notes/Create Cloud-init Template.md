---
categories:
  - "[[Guides]]"
topics:
  - "[[Proxmox]]"
  - "[[Linux]]"
created:
---


Creates a base Ubuntu image template on proxmox host that will be used to deploy other VM's using Ansible.

``` shell
$ ansible-playbook create_cloud_init_template.yml -i ../inventory.yml --ask-vault-pass
```

