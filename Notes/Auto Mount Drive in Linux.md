---
categories:
  - "[[Guides]]"
topics:
  - "[[Linux]]"
created:
---


Locate the name of the drive to be mounted:
```shell
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   64G  0 disk
├─sda1   8:1    0   63G  0 part /
├─sda2   8:2    0    1K  0 part
└─sda5   8:5    0  975M  0 part [SWAP]
sdb      8:16   0 14.6T  0 disk
└─sdb1   8:17   0 14.6T  0 part /mnt/ext1
sr0     11:0    1  940M  0 rom
```

Create a mount point:
```shell
$ mkdir /mnt/ext1
$ sudo chown -R $USER:USER /mnt/ext1
```

Create entry in `/etc/fstab`:
```shell
echo "/dev/sdb1 /mnt/ext1 ext4 defaults 0 0" >> /etc/fstab
```

Test the mount, if no errors occur then everything is working properly:
```shell
$ mount -a
```
