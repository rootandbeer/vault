---
categories:
  - "[[Guides]]"
topics:
  - "[[Docker]]"
  - "[[Hugo]]"
created:
---


```shell
version: '3'

services:

  ghost:
    image: ghost:latest
    restart: always
    environment:
      database__client: mysql
      database__connection__host: db
      database__connection__database: ghost
      database__connection__user: ghost
      database__connection__password: "REDACTED"
      url: https://www.rootandbeer.com
      # mail__transport: "SMTP"
      # mail__options__host: "${MAIL_HOST}"
      # mail__options__port: "${MAIL_PORT}"
      # mail__options__secureConnection: "${MAIL_SECURE_CONNECTION}"
      # mail__options__auth__user: "name@website.com"
      # mail__options__auth__pass: ""
    ports:
      - "2370:2368"
    volumes:
      - ghost-data:/var/lib/ghost/content
      - ghost-config:/var/lib/ghost
    links:
      - db

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: "REDACTED"
      MYSQL_DATABASE: ghost
      MYSQL_USER: ghost
      MYSQL_PASSWORD: "REDACTED"
    cap_add:
      - SYS_NICE
    volumes:
      - ghost-db:/var/lib/mysql

volumes: 
  ghost-data:
  ghost-db:
  ghost-config:

networks:
  ghost-network:
  
```
  