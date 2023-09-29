---
layout: post
title: Netbox Install
subtitle: Netboxのインストール手順
gh-repo: junenu
gh-badge: [follow]
tags: [netbox, docker]
comments: true
---
# インストール手順
## 必要なパッケージのインストール

```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

## Dockerのdownload
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Dockerのインストール
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## dockerをsudo無しで実行するための設定
```bash
getent group | grep docker
sudo usermod -aG docker $USER
getent group | grep docker
```

## dockerサービスの再起動
```bash
sudo service docker stop
sudo service docker start
```

## Netboxのコンテナ起動
```bash
git clone -b release https://github.com/netbox-community/netbox-docker.git
cd netbox-docker/
tee docker-compose.override.yml <<EOF
version: '3.4'
services:
  netbox:
    ports:
      - 8000:8080
EOF
docker compose pull
docker compose up -d
```
