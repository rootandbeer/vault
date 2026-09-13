---
categories:
  - "[[Guides]]"
topics:
created:
---


To run website locally, go to hugo home dir:
```shell
docker compose up
```

For a new post (you do NOT need to add `/content` folder):
```shell
hugo new blog/test123/index.md
```


example callouts:
https://bootstrap.hugomods.com/docs/alert/
```
{{< bs/alert >}} {{< markdownify >}} **Option 1:** RCE via 
content here
content here2
{{< /markdownify >}} {{< /bs/alert >}}
```

  ```shell
{{< bs/alert danger >}} {{< markdownify >}} **Option 1:** RCE via 
content here
content here2
{{< /markdownify >}} {{< /bs/alert >}}
  ```


```

---
## Related Links
- https://hbs.razonyang.com/v1/en/docs/content/
