---
categories:
  - "[[Guides]]"
topics:
  - "[[Linux]]"
created:
---

mounting proton drive on boot using rclone 

```bash
sudo nano /etc/systemd/system/protondrive.service                  
```

add the following:

```bash
[Unit]
Description=rclone for <rclone_mount_name>
After=network-online.target
Wants=network-online.target
[Service]
#Type=simple
User=<UID>
ExecStart=rclone mount <rclone_mount_name>: /mnt/<mounted_folder> --allow-other --protondri>
ExecStop=/bin/fusermount -u /mnt/<mounted_folder>
Restart=always
#RestartSec=10

[Install]
WantedBy=default.target
```

