---
categories:
  - "[[Guides]]"
topics:
  - "[[Docker]]"
created:
---


```shell
$ docker stop jellyfin

$ cd /portainer/Files/AppData/Config/Jellyfin/data/data

$ mv jellyfin.db jellyfin.db.bak



$ sudo find / -name system.xml
./Jellyfin/system.xml

```

Edit the line : `<IsStartupWizardCompleted>true</IsStartupWizardCompleted>` to `false`

```shell
$ docker start jellyfin
```

Visit the WebUI to begin configuration (may have to give it a few minutes for the container to start)