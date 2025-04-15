---
sidebar_position: 5
draft: false
---

# Peertube

## Зависимости

⚠️ Внимание : руководство по зависимостям поддерживается сообществом. Некоторые части могут быть устаревшими! ⚠️

Основные зависимости, поддерживаемые PeerTube:

`node` LTS (>= 20.9 or 22.x)

`yarn` 1.x (не должно быть >=2.x)

`postgres` >=10.x

`redis-server` >=6.x

`ffmpeg` >=4.3 (использование статической сборки `ffmpeg` не рекомендуется)

`python` >=3.8

`pip`

примечание : поддерживаются только версии LTS внешних зависимостей. Если версия LTS, соответствующая ограничению версии, недоступна, поддерживаются только версии релиза .

```
sudo apt-get install curl sudo unzip vim
sudo apt install certbot nginx ffmpeg postgresql postgresql-contrib openssl g++ make redis-server git cron wget
g++ -v

// OR
sudo apt-get install curl unzip openssl g++ make git cron wget
```


Посмотреть список пользователей, подключенных к серверу в данный момент, можно при помощи команды `who`

Если пользователь, которого вы хотите удалить, авторизован на сервере, проверьте, какие процессы им запущены. Если какие-то операции выполняются пользователем в фоновом режиме, вы не сможете удалить учетную запись. Посмотреть список запущенных процессов можно с помощью команды:

```
sudo ps -u peertube
```

Далее вам нужно заблокировать доступ пользователя на сервер. Для этого введите команду:

```
sudo passwd -l peertube
```

Удаление процессов

В операционной системе Ubuntu невозможно удалить учетную запись пользователя, если им запущены какие-либо процессы. Завершить запущенные процессы можно с помощью команд:

`kill` — используется для удаления процессов по их идентификатору. Чтобы узнать идентификатор процесса, воспользуйтесь командой `sudo ps -u username`, где `username` — имя пользователя. Идентификатор будет отображаться в графе `PID`. Например, чтобы удалить процесс с `PID 12345`, нужно ввести команду: `sudo kill 12345`

`pkill` — используется для удаления процессов по их названию. Например, чтобы удалить процесс с именем nano у пользователя username, нужно ввести команду: `sudo pkill nano -u username`

`killall` — используется для удаления всех процессов, включая дочерние. В случае, когда вы собираетесь удалить пользователя, удобнее всего использовать эту команду. Рекомендуем добавить к команде ключ 9 — тогда процессы получат сигнал SIGKILL и будут принудительно завершены. Например, чтобы удалить все процессы для пользователя `username`, нужно ввести команду: sudo killall -9 -u peertube



Пользователь PeerTube
Создайте peertube пользователя с /var/www/peertube домашним адресом:

```
sudo useradd -m -d /var/www/peertube -s /usr/sbin/nologin -p peertube peertube

sudo chown peertube:peertube /var/www/peertube
```

Установите пароль:

```
sudo passwd peertube
```

База данных

Создайте производственную базу данных и пользователя `peertube` внутри `PostgreSQL`:

```
cd /var/www/peertube
sudo -u postgres createuser -P peertube
```

Здесь вы должны ввести пароль для peertubeпользователя PostgreSQL, который должен быть скопирован в production.yaml файл. Не нажимайте просто Enter, иначе он будет пустым.


`sudo -u postgres createdb -O peertube -E UTF8 -T template0 peertube_prod`

Затем включите расширения, необходимые PeerTube:

```
sudo -u postgres psql -c "CREATE EXTENSION pg_trgm;" peertube_prod
sudo -u postgres psql -c "CREATE EXTENSION unaccent;" peertube_prod
```

Подготовьте каталог PeerTube

Загрузите последнюю версию Peertube с тегами:

```
VERSION=$(curl -s https://api.github.com/repos/chocobozzz/peertube/releases/latest | grep tag_name | cut -d '"' -f 4) && echo "Latest Peertube version is $VERSION"
```

Откройте каталог peertube, создайте несколько необходимых каталогов:

```
cd /var/www/peertube
sudo -u peertube mkdir config storage versions
sudo -u peertube chmod 750 config/
```

Загрузите последнюю версию клиента Peertube, распакуйте ее и удалите zip-архив:


cd /var/www/peertube/versions
# Releases are also available on https://builds.joinpeertube.org/release
sudo -u peertube wget -q "https://github.com/Chocobozzz/PeerTube/releases/download/${VERSION}/peertube-${VERSION}.zip"
sudo -u peertube unzip -q peertube-${VERSION}.zip && sudo -u peertube rm peertube-${VERSION}.zip

Установить Peertube:

```
cd /var/www/peertube
sudo -u peertube ln -s versions/peertube-${VERSION} ./peertube-latest

cd ./peertube-latest
//sudo -H -u peertube npm run install-node-dependencies -- --production
npm run install-node-dependencies -- --production

```

Конфигурация PeerTube
Скопируйте файл конфигурации по умолчанию, содержащий конфигурацию по умолчанию, предоставленную PeerTube. Вы не должны обновлять этот файл.

```
cd /var/www/peertube
sudo -u peertube cp peertube-latest/config/default.yaml config/default.yaml
```

Теперь скопируйте конфигурацию примера производства:

```
cd /var/www/peertube
sudo -u peertube cp peertube-latest/config/production.yaml.example config/production.yaml
```

Затем отредактируйте `config/production.yaml` файл в соответствии с конфигурацией вашего веб-сервера и базы данных. В частности:

