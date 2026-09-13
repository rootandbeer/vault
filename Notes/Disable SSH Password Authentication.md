---
categories:
  - "[[Guides]]"
topics:
  - "[[SSH]]"
  - "[[Linux]]"
created:
---


```shell
sudo nano /etc/ssh/sshd_config
```

Ensure that these lines are set:
```shell
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart ssh:
```shell
sudo systemctl restart ssh
```