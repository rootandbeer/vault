---
categories:
  - "[[Guides]]"
topics:
  - "[[Docker]]"
  - "[[Kasm]]"
url: https://kasmweb.atlassian.net/wiki/spaces/KCS/pages/82640932/DRAFT+How-to+Backup+and+Restore+a+Kasm+deploy+to+a+different+server
created: 2025-08-24
---
This article focuses on how-to migrate a kasm single server deployment from production to staging. Migrating a multiple server kasm architecture is similar but outside of the scope of this document.

The examples provided assume the latest version of Kasm is version 1.15.0. The versions and download ULRs should be changed to reflect the versions of kasm in use in your deployment.

## ![blue book](https://pf-emoji-service--cdn.us-east-1.prod.public.atl-paas.net/standard/ef8b0642-7523-4e13-9fd3-01b65648acf6/32x32/1f4d8.png) Single-Server Migration

1. **Create a Database Backup (Excluding all kasm logs)**
    
```bash
sudo /opt/kasm/bin/stop sudo mkdir -p /opt/kasm/backups/ 
sudo bash /opt/kasm/<PREVIOUS_VERSION>/bin/utils/db_backup -f /opt/kasm/backups/kasm_db_backup.tar -p /opt/kasm/<PREVIOUS_VERSION>/ 
sudo ls -al /opt/kasm/backups/kasm_db_backup.tar
```
    
2. **Copy the Database Backup to the new server.**  
Below is an example of using SCP on a bastion host to copy the file from the source to the bastion, from the bastion to the destination, then deleting the file from the bastion.
    
```shell
scp user@source_hostname:/opt/kasm/backups/kasm_db_backup.tar ./kasm_db_backup.tar 
scp ./kasm_db_backup.tar user@dest_hostname:/opt/kasm/backups/kasm_db_backup.tar rm ./kasm_db_backup.tar
```
    
3. **Download and extract the new installation media**
    
```bash
cd /tmp curl -O https://kasm-static-content.s3.amazonaws.com/<KASM_INSTALLATION_ARCHIVE>.tar.gz tar -xf <KASM_INSTALLATION_ARCHIVE>.tar.gz
```
    
4. **Get the existing database password for use in the subsequent commands.**
    
    `sudo grep "password" /opt/kasm/<PREVIOUS_VERSION>/conf/app/api.app.config.yaml`
    
5. **Get the existing manager token for use in the subsequent commands.**
    
    `sudo grep "token" /opt/kasm/<PREVIOUS_VERSION>/conf/app/agent.app.config.yaml`
    
6. **Perform a clean install**
    
    `sudo bash kasm_release/install.sh -D -Q <DATABASE_PASSWORD> -R <REDIS_PASSWORD> -M <MANAGER_TOKEN>`
    
7. **Modify Configs - Copy the server_id and the public_hostname properties from the old agent to the new**
    
```shell
grep server_id /opt/kasm/<PREVIOUS_VERSION>/conf/app/agent.app.config.yaml
grep public_hostname /opt/kasm/<PREVIOUS_VERSION>/conf/app/agent.app.config.yaml 
sudo vi /opt/kasm/<NEW_VERSION>/conf/app/agent.app.config.yaml
```
    
8. **Modify Configs - Copy manager_id and server_hostname from the old configuration into the new**
    
```shell
grep manager_id /opt/kasm/<PREVIOUS_VERSION>/conf/app/api.app.config.yaml
grep server_hostname /opt/kasm/<PREVIOUS_VERSION>/conf/app/api.app.config.yaml
sudo vi /opt/kasm/<NEW_VERSION>/conf/app/api.app.config.yaml
```
    
9. **Modify Configs - Copy the id and the token properties from the old connection_proxy to the new**
    
```shell
grep id /opt/kasm/<PREVIOUS_VERSION>/conf/app/kasmguac.app.config.yaml
grep token /opt/kasm/<PREVIOUS_VERSION>/conf/app/kasmguac.app.config.yaml
sudo vi /opt/kasm/<NEW_VERSION>/conf/app/kasmguac.app.config.yaml
```
    
10. **Restoring the Database (x86 & amd64).**
    
 ```shell
 sudo /opt/kasm/bin/stop sudo /opt/kasm/<NEW_VERSION>/bin/utils/db_restore -f /opt/kasm/backups/kasm_db_backup.tar -p /opt/kasm/<NEW_VERSION> sudo /opt/kasm/bin/start
```