---
categories:
  - "[[Guides]]"
topics:
  - "[[Proxmox]]"
created:
---


In proxmox UI:
select node-->system-->network

click **create** --> **Linux Bridge**

Enter name (eg. vmbr1) and bridge ports (can be found using `lsusb`)

then click save.

Click **Apply Configuration** at the top of the window

select vendor name

add the network device
- for model try `Virtio` or `E1000`
