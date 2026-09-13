---
categories:
  - "[[Guides]]"
topics:
  - "[[OMV]]"
  - "[[Linux]]"
created:
---

### ⚙️ **1. Prepare your shared folder**

✅ **Create the shared folder to hold your media:**
1. Log into **OpenMediaVault web interface**.
2. Go to **Storage → File Systems**.
    - Ensure your storage drive is mounted. (or mount it with the ">" icon)
3. Go to **mergerfs** --> **Create**
4. Go to **Storage → Shared Folders**.
5. Click **“Add”**.
    - **Name:** media
    - **Device:** your mounted drive
    - **Path:** auto-generated or customize if needed
    - **Permissions:** set as **Everyone: read/write** initially for testing, then lock down later based on user needs.
### 🔗 **2. Enable and configure NFS**

✅ **Enable the NFS plugin/service:**
1. Go to **Services → NFS**.
2. Click **“Enable”**, then **“Save” and “Apply.”**

✅ **Create an NFS share:**
1. Under **Services → NFS → Shares**, click **“Add.”**
2. **Shared folder:** select your media folder.
3. **Client:**
    - For Jellyfin, specify the IP address or subnet (e.g. `192.168.1.10` or `192.168.1.0/24`).
4. **Extra options:**
    - **Read/Write:** enable if Jellyfin needs to write metadata, otherwise Read-only is safer.
    - **Sync:** keep checked for data integrity.
    - **Extra Options:** subtree_check,insecure,no_root_squash
5. Click **“Save”** and **“Apply.”**

✅ **Test mounting on Jellyfin server (Linux client):**
`sudo mkdir -p /mnt/media sudo` 
`mount -t nfs <NAS_IP>:/export/media /mnt/media`

Replace `<NAS_IP>` with your OMV server IP. Adjust path if your export differs.

➡️ **Make it persistent** by adding to `/etc/fstab`:
`<NAS_IP>:/export/media /mnt/media nfs defaults 0 0`

---

