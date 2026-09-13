---
categories:
  - "[[Projects]]"
topics:
  - "[[Ragnar]]"
created:
---

   
   Important notes:
1. If configuring Windows PC for USB gadget connection:
   - Set static IP: 172.20.2.2
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 172.20.2.1
   - DNS Servers: 8.8.8.8, 8.8.4.4
2. Web interface will be available at: http://[device-ip]:8000

AP Mode:
SSID: Ragnar
pass: ragnarconnect
portal: http://192.168.4.1/portal
   
   cd /home/ragnar/Ragnar
   sudo git stash  # Save any local changes
   sudo git pull   # Get latest updates
   sudo systemctl restart ragnar