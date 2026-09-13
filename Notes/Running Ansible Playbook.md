---
categories:
  - "[[Guides]]"
topics:
  - "[[References/Ansible]]"
created:
---


If user does NOT require sudo password:
```shell
ansible-playbook -i inventory.yml <playbook>.yml
```

If user requires sudo password:
```shell
ansible-playbook -i inventory.yml <playbook>.yml --ask-become-pass
```

