---
categories:
  - "[[Guides]]"
topics:
  - "[[OMV]]"
  - "[[Linux]]"
created:
---


The OMV extras plugin needs to be installed to access MergerFS and SnapRAID. In the OMV VM run the following:

```shell
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash
```

Then head back to the Web UI, go to **System** then **Plugins**. Search for **MergerFS** and **SnapRAID**. Click to install.