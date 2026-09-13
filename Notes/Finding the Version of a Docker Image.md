---
categories:
  - "[[Guides]]"
topics:
  - "[[Docker]]"
created:
---


Sometimes the version of a docker image is not represented in the tag. For example if using the `latest` tag the version will not be represented.

```bash
docker login <registry_url>
docker pull <registry_url>/<repo-key>/<image-name>:<tag>
docker images
```

>[!tip]
>For Docker Hub use the URL: docker.io

