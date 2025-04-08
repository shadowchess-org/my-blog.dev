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

