---
sidebar_position: 2
---

# Tools

## Ubuntu

```
sudo apt update
sudo apt upgrade

sudo apt -y install build-essential git gcc libssl-dev curl cron make pkg-config


```

## Rust

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh


This is usually done by running one of the following (note the leading DOT):
. "$HOME/.cargo/env"            # For sh/bash/zsh/ash/dash/pdksh
source "$HOME/.cargo/env.fish"  # For fish
source "$HOME/.cargo/env.nu"    # For nushell

rustc --version

rustup update

```

## Nodejs

```
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 22

# Verify the Node.js version:
node -v # Should print "v22.14.0".
nvm current # Should print "v22.14.0".

# Download and install pnpm:
corepack enable pnpm

# Verify pnpm version:
pnpm -v

```

## Docker

Чтобы удалить все конфликтующие пакеты, выполните следующую команду:

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Настройте aptрепозиторий Docker.

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Чтобы установить последнюю версию, выполните:

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


Service restarts being deferred:
 /etc/needrestart/restart.d/dbus.service
 systemctl restart networkd-dispatcher.service
 systemctl restart systemd-logind.service
 systemctl restart unattended-upgrades.service

```

Убедитесь, что установка прошла успешно, запустив hello-world образ:

```
sudo docker run hello-world
```


```
cd /var/www/sites
git clone https://github.com/shadowchess-org/my-blog.dev.git

cd /var/www/sites/my-blog.dev
pnpm install
pnpm build

cd /var/www/sites/my-blog.dev
git pull https://github.com/shadowchess-org/my-blog.dev.git
pnpm build
```

