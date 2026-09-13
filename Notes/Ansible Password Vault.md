---
categories:
  - "[[Guides]]"
topics:
  - "[[References/Ansible]]"
created:
---
## New Vault

Create a vault by typing
```shell
$ ansible-vault create secrets.yml
```

Create a password for the vault then a Vim window will pop up. type `:q` then hit `ENTER` to quit.

Verify encryption:
```shell
$ cat secrets.yml
```

## Adding Secret to existing vault

To add a secret type:
```shell
$ ansible-vault edit secrets.yml
```

the Vim window will pop up and type `i` to make edits and once done type `:wq` to save and quit

## Using Secrets

Reference secret in playbook example:
```shell
...
vars_files:
  - secrets.yml

vars:
  api_password: "{{ password_title }}"
...
```

