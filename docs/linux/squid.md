---
sidebar_position: 4
draft: false
---

# Squid

```
squid -v
sudo apt-get remove squid
sudo apt-get purge squid
sudo rm -rf /var/spool/squid
```

### Как удалить прокси-сервер Squid

Если вы хотите удалить Squid Proxy Server с вашего компьютера Ubuntu, выполните следующие действия:

### Шаг 1: Остановите службу прокси-сервера Squid

Перед удалением Squid Proxy Server необходимо остановить службу Squid Proxy, введя следующую команду:

```
sudo systemctl stop squid
```

### Шаг 2: Удалить пакет Squid Proxy

Удалите пакет Squid Proxy с вашего компьютера Ubuntu, введя следующую команду:

```
sudo apt-get remove squid
```

Эта команда удалит пакет Squid Proxy из вашей системы, но не удалит никакие файлы конфигурации или данные, связанные с пакетом.

### Шаг 3: Удалите файлы конфигурации прокси-сервера Squid

Если вы хотите удалить файлы конфигурации Squid Proxy, введите следующую команду:

```
sudo apt-get purge squid
```

Эта команда удалит пакет Squid Proxy и все связанные с ним файлы конфигурации и данные из вашей системы.

### Шаг 4: Удалить кэш прокси-сервера Squid

Если вы хотите удалить кэш Squid Proxy, введите следующую команду:

```
sudo rm -rf /var/spool/squid
```

Эта команда удалит каталог кэша Squid Proxy и все его содержимое.

### Шаг 5: Перезагрузите ваш компьютер Ubuntu.

Перезагрузите компьютер Ubuntu, введя следующую команду:

```
sudo reboot
```

## Install

```
sudo apt install squid -y
squid -v
sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.backup
```

287.103.197.160

squid.conf

```
http_port 3128
acl localnet src 287.103.197.160/24
http_access allow localnet
http_access deny all

cache_dir ufs /var/spool/squid 100 16 256
maximum_object_size 4096 KB
minimum_object_size 0 KB

access_log /var/log/squid/access.log
cache_log /var/log/squid/cache.log
```


```
sudo systemctl restart squid
sudo systemctl status squid
```

3128

180.209.243.9

166.151.40.150