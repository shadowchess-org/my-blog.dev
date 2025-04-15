---
sidebar_position: 6
draft: false
---

# PostgreSQL

В этом руководстве мы рассмотрим, как установить PostgreSQL 17 на Ubuntu 24.04. Мы также рассмотрим некоторые базовые настройки, чтобы разрешить удаленные подключения, включить аутентификацию по паролю и начать создавать пользователей, базы данных и т. д.

Предварительные требования

`Ubuntu` 24.04

Права `root` или доступ `sudo`

для `sudo su` входа в систему как `root` вместо `ubuntu` (пользователь по умолчанию)

## Шаг 1 — Добавление репозитория PostgreSQL

Сначала обновите индекс пакетов и установите необходимые пакеты:

```
sudo apt update
```

Добавьте репозиторий PostgreSQL 17:

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

Импортируйте ключ подписи репозитория:

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

Обновите список пакетов:

```
sudo apt update
```

## Шаг 2 — Установка PostgreSQL 17

Установите PostgreSQL 17 и дополнительные модули:

```
sudo apt install postgresql-17
```

Запустите и включите службу PostgreSQL:

```
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

Проверьте версию и убедитесь, что это Postgresql 17:

`psql --version`

Вы должны получить что-то вроде

`psql (PostgreSQL) 17.4 (Ubuntu 17.4-1.pgdg24.04+2)`

## Шаг 3 — Настройка PostgreSQL 17

Отредактируйте `postgresql.conf`, чтобы разрешить удаленные подключения, изменив `listen_addresses` на `*`:

```
sudo nano /etc/postgresql/17/main/postgresql.conf
listen_addresses = '*'
```

Настройте `PostgreSQL` для использования аутентификации по паролю `md5`, отредактировав `pg_hba.conf`. Это важно, если вы хотите подключаться удаленно, например, через `PGADMIN` :

```
sudo sed -i '/^host/s/ident/md5/' /etc/postgresql/17/main/pg_hba.conf
sudo sed -i '/^local/s/peer/trust/' /etc/postgresql/17/main/pg_hba.conf
echo "host all all 0.0.0.0/0 md5" | sudo tee -a /etc/postgresql/17/main/pg_hba.conf
```

Перезапустите PostgreSQL, чтобы изменения вступили в силу:

```
sudo systemctl restart postgresql
```

Разрешить порт PostgreSQL через брандмауэр:

```
sudo ufw allow 5432/tcp
```

## Шаг 4 — Подключение к PostgreSQL

Подключитесь как пользователь postgres:

```
sudo -u postgres psql
```

Установите пароль для пользователя `postgres`:

```
ALTER USER postgres PASSWORD 'admin@password';
```

После этого можно выйти из консоли PostgreSQL:

```
\q
```