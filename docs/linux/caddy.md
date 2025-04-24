---
sidebar_position: 1
---

# Caddy

[caddyserver.com](https://caddyserver.com/docs/install#debian-ubuntu-raspbian)

Stable releases:

```

sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

Testing releases (includes betas and release candidates):

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/testing/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-testing-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/testing/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-testing.list
sudo apt update
sudo apt install caddy
```


```
caddy version
v2.10.0 h1:fonubSaQKF1YANl8TXqGcn4IbIRUDdfAkpcsfI/vX5U=

/etc/caddy/Caddyfile

www1.site.dev {
	encode zstd gzip
	reverse_proxy localhost:8001
}


www2.site.dev {
  root * /var/www/dev/www2.site.dev/public
  file_server
}

systemctl reload caddy

systemctl status caddy

```