---
categories:
  - "[[Guides]]"
topics:
  - "[[Docker]]"
created:
---


Source: https://stackoverflow.com/questions/18274088/how-can-i-make-my-own-base-image-for-docker

1. Create the tar files for your file system, simply could be
    ```shell
    tar --numeric-owner --exclude=/proc --exclude=/sys -cvf centos6-base.tar /
    ```
2. Transfer the tar file to other docker system if not installed locally and `import` it
    ```shell
    cat centos6-base.tar | docker import - centos6-base
    ```
3. Now you can verify by running it.
    ```shell
    docker run -i -t centos6-base cat /etc/redhat-release
    ```