---
categories:
  - "[[Guides]]"
topics:
  - "[[Linux]]"
created:
---


### SCP:
Local File to Remote Machine:

```shell
scp /path/to/local/file username@remote_host:/path/to/remote/destination
```

Download remote file to local machine:
```shell
$ scp <USER>@<REMOTE_IP>:<REMOTE_FILE> .
```

### SSH:
On local machine:
```shell
$ ssh <USER>@<REMOTE_IP> 'cat <REMOTE_FILE>' > <LOCAL_FILE>
```

### Python Web Server
On remote machine:
```shell
$ python -m http.server <PORT>
```

Then on local machine in a web browser access: `http://<WEBSERVER_IP>:<PORT>`