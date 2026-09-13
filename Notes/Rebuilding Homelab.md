---
categories:
  - "[[Guides]]"
topics:
  - "[[Proxmox]]"
  - "[[References/Ansible]]"
created:
---


1. [[Setting Up SSH Keys]] - Save as `servers` & `servers.pub`
2. [[Setting up Proxmoxer venv]]
3. [[Ansible Password Vault]]
	1. Vault should have these entries (**NOTE:** the portainer username and password entries will setup the username and password for portainer server autonomously): 
		1. **proxmox_api_password**: password set when setting up proxmox
		2. **ssh_pub_key:** public key from step one
		3. **portainer_username**: set username for portainer server 
		4. **portainer_password:** set password for portainer server
		5. **github_pat:** personal access token from github
4. Add **portainer_username** & **portainer_password** from above to github secrets (github-->repo-->settings-->secrets-->actions)

5. [[Create Cloud-init Template]]
6. Deploy VMs:
   ```shell
$ ansible-playbook //homelab/ansible/playbooks/deploy_vms_from_template.yml -i //homelab/ansible/inventory.yml --ask-vault-pass
```
6. Modify VM resources:
   ```shell
$ ansible-playbook //homelab/ansible/playbooks/modify_proxmox_vm_resources.yml -i //homelab/ansible/inventory.yml --ask-vault-pass
```


