---
layout: post
title: "Code Server: Your Next Code Editor"
date: 2022-12-09 21:17 +0800
category: [tools]
---
## Preparation

1. A 2C/4GB  Linux server with a binding domain
2. Operating System: Ubuntu 22.04

## Installation

### Install code-server in Linux

> Execute following script, the version is the release of code server in GitHub

```shell
curl -fOL https://github.com/coder/code-server/releases/download/v$VERSION/code-server_${VERSION}_amd64.deb
sudo dpkg -i code-server_${VERSION}_amd64.deb
sudo systemctl enable --now code-server@$USER
# Now visit http://127.0.0.1:8080. Your password is in ~/.config/code-server/config.yaml

```

### Modify `.config/code server/config.yaml`

```yaml
bind-addr: 0.0.0.0:8080
auth: password
password: xxx # your password
cert: false
```

### Open https using Nginx and Certbot

Install nginx

```shell
sudo apt update
sudo apt install -y nginx certbot python3-certbot-nginx
```

Create nginx config with following code, and replace mydomain.com with your real domain

```shell
server {
    listen 80;
    listen [::]:80;
    server_name mydomain.com;

    location / {
      proxy_pass http://localhost:8080/;
      proxy_set_header Host $host;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection upgrade;
      proxy_set_header Accept-Encoding gzip;
    }
}

```

Enable the config, replace domain and email as your real domain and email

```shell
sudo ln -s ../sites-available/code-server /etc/nginx/sites-enabled/code-server
sudo certbot --non-interactive --redirect --agree-tos --nginx -d mydomain.com -m me@example.com
```

Finally, reload nginx and open your domain in browser, enjoy code server!

## Use Code Server with iPad

1. Using code server in browser
2. Save the page as shortcut in home screen
