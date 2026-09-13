---
categories:
  - "[[Guides]]"
topics: []
url:
created: 2026-09-08
---
## Finding Ansible Modules and Parameters

### 1. Find modules locally
```bash
source ~/ansible-venv/bin/activate
ansible-doc -l # all installed modules
ansible-doc -l | grep docker # filter by keyword
ansible-galaxy collection list # see installed collections
```

If a module is missing, install its collection:
```
ansible-galaxy collection install community.docker
```
### 2. Read module parameters
```bash
ansible-doc apt # full docs + examples
ansible-doc -s apt # parameter schema only
ansible-doc community.docker.docker_compose_v2
```

If `ansible-doc` says “module not found”, the collection is not installed.
### 3. Find existing solutions (optional)

Galaxy roles — reuse install logic instead of writing from scratch:
```bash
ansible-galaxy search docker
ansible-galaxy info geerlingguy.docker
```

Check the role’s `tasks/main.yml` on GitHub for working patterns.

Web search — targeted queries work best:

- `site:docs.ansible.com docker compose`
- `site:github.com ansible install docker ubuntu playbook`

Prefer examples that match your OS (Ubuntu 24.04) and style (plain tasks vs roles).
### 4. Map manual steps to modules

Start with what you’d run by hand:

```bash
ssh ubuntu@192.168.3.50
# apt install docker...
docker compose up -d
```

Then match each step to a module:

|Manual step|Module|
|---|---|
|`apt install ...`|`ansible.builtin.apt`|
|Add apt repo|`ansible.builtin.apt_repository` or `deb822_repository`|
|`systemctl enable docker`|`ansible.builtin.service`|
|Copy compose file|`ansible.builtin.copy` or `template`|
|`docker compose up -d`|`command`/`shell`, or `community.docker.docker_compose_v2`|

Stuck? Run `ansible-doc -l | grep <keyword>`.
### 5. Test before writing a full playbook

```bash
ansible standalone_vms -m ping
ansible standalone_vms -m apt -a "name=curl state=present" -b
ansible-playbook playbooks/configure_standalone_vm.yml --check --diff
```

- `--check` — preview changes without applying
- `--diff` — show file diffs
### Quick reference

|Goal|Command|
|---|---|
|List modules|`ansible-doc -l \| grep <keyword>`|
|Module parameters|`ansible-doc <module>`|
|Installed collections|`ansible-galaxy collection list`|
|Existing roles|`ansible-galaxy search <keyword>`|
|Official docs|[docs.ansible.com module index](https://docs.ansible.com/ansible/latest/collections/index_module.html)|

---
