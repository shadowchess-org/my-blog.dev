---
sidebar_position: 3
---

# Forgejo


```
sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' --group --disabled-password --home /home/git git


info: Selecting UID from range 100 to 999 ...

info: Selecting GID from range 100 to 999 ...
info: Adding system user `git' (UID 1000) ...
info: Adding new group `git' (GID 1001) ...
info: Adding new user `git' (UID 1000) with group `git' ...
info: Creating home directory `/home/git' ...


```

Необходимо запомнить `UID (1000)` и `GID (1001)`, чтобы заменить эти значения в файле `docker-compose.yml`

Для разрешения к docker

`sudo usermod -aG docker git`

`groups git`

`mkdir ~/forgejo`

`cd ~/forgejo`

docker-compose.yml

```
networks:
  forgejo:
    external: false

services:
  server:
    image: data.forgejo.org/forgejo/forgejo:10
    container_name: forgejo
    environment:
      - USER_UID=1000
      - USER_GID=1001
      - FORGEJO__database__DB_TYPE=postgres
      - FORGEJO__database__HOST=db:5432
      - FORGEJO__database__NAME=forgejo
      - FORGEJO__database__USER=forgejo
      - FORGEJO__database__PASSWD=forgejo
    restart: always
    networks:
      - forgejo
    volumes:
      - ./forgejo:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "8001:3000"
      - "222:22"
    depends_on:
      - db

  db:
    image: postgres:17
    restart: always
    environment:
      - POSTGRES_USER=forgejo
      - POSTGRES_PASSWORD=forgejo
      - POSTGRES_DB=forgejo
    networks:
      - forgejo
    volumes:
      - ./postgres:/var/lib/postgresql/data

```

Если `codeberg.org` доступ невозможен, вы можете заменить каждое упоминание на  `data.forgejo.org` и использовать наше зеркало.

`sudo docker compose up -d`

Обновление

```
docker compose pull
docker compose up --force-recreate --build -d
docker image prune -f
```
