---
categories:
  - "[[Guides]]"
topics:
  - "[[SSH]]"
  - "[[Linux]]"
created:
---


In command prompt on client machine type:
```shell
$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (~/.ssh/id_ed25519): ~/.ssh/<keyname>
```
>[!caution] Make sure you rename the key in the path above when prompted

Set permission
```shell
$ chmod 600 ~/.ssh/<keyname>
```

Add entry into the `~/.ssh/config` file:
```shell
Host <hostname>
  HostName <server_ip>
  User <username>
  IdentityFile C:\Users\robwr\.ssh\<keyname>
```

Copy contents of `~/.ssh/<keyname>.pub`

Login in the remote machine then type:
```shell
$ echo <paste_contents> >> ~/.ssh/authorized_keys
```

Confirm that SSHD config allows key auth
```shell
$ sudo nano /etc/ssh/sshd_config
```

Ensure these lines are present:
```shell
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

Restart SSH
```shell
$ sudo systemctl restart ssh
```


## Related Links
- [[Disable SSH Password Authentication]]