---
layout: post
title: Netbox Install
subtitle: 手順と作業ログ
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

# 作業ログ
```bash
junenu@randall:~$ sudo apt-get update
Hit:1 http://jp.archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://jp.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:4 http://security.ubuntu.com/ubuntu jammy-security/main amd64 DEP-11 Metadata [43.1 kB]
Get:5 http://jp.archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]
Get:6 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 DEP-11 Metadata [40.1 kB]
Get:7 http://jp.archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1,012 kB]
Get:8 http://jp.archive.ubuntu.com/ubuntu jammy-updates/main i386 Packages [494 kB]
Get:9 http://jp.archive.ubuntu.com/ubuntu jammy-updates/main amd64 DEP-11 Metadata [101 kB]
Get:10 http://jp.archive.ubuntu.com/ubuntu jammy-updates/main amd64 c-n-f Metadata [15.6 kB]
Get:11 http://jp.archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [900 kB]
Get:12 http://jp.archive.ubuntu.com/ubuntu jammy-updates/restricted i386 Packages [31.9 kB]
Get:13 http://jp.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [987 kB]
Get:14 http://jp.archive.ubuntu.com/ubuntu jammy-updates/universe i386 Packages [657 kB]
Get:15 http://jp.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 DEP-11 Metadata [289 kB]
Get:16 http://jp.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 c-n-f Metadata [21.9 kB]
Get:17 http://jp.archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [41.6 kB]
Get:18 http://jp.archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 DEP-11 Metadata [940 B]
Get:19 http://jp.archive.ubuntu.com/ubuntu jammy-backports/main amd64 DEP-11 Metadata [4,952 B]
Get:20 http://jp.archive.ubuntu.com/ubuntu jammy-backports/universe amd64 DEP-11 Metadata [17.8 kB]
Fetched 4,995 kB in 3s (1,765 kB/s)
Reading package lists... Done
junenu@randall:~$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
lsb-release is already the newest version (11.1.0ubuntu4).
lsb-release set to manually installed.
curl is already the newest version (7.81.0-1ubuntu1.13).
gnupg is already the newest version (2.2.27-3ubuntu2.1).
gnupg set to manually installed.
The following package was automatically installed and is no longer required:
  systemd-hwe-hwdb
Use 'sudo apt autoremove' to remove it.
The following packages will be upgraded:
  ca-certificates
1 upgraded, 0 newly installed, 0 to remove and 417 not upgraded.
Need to get 0 B/155 kB of archives.
After this operation, 15.4 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Preconfiguring packages ...
(Reading database ... 203437 files and directories currently installed.)
Preparing to unpack .../ca-certificates_20230311ubuntu0.22.04.1_all.deb ...
Unpacking ca-certificates (20230311ubuntu0.22.04.1) over (20211016) ...
Setting up ca-certificates (20230311ubuntu0.22.04.1) ...
Updating certificates in /etc/ssl/certs...
rehash: warning: skipping ca-certificates.crt,it does not contain exactly one certificate or CRL
19 added, 9 removed; done.
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for ca-certificates (20230311ubuntu0.22.04.1) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
junenu@randall:~$ sudo mkdir -p /etc/apt/keyrings
junenu@randall:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
junenu@randall:~$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
junenu@randall:~$ sudo apt-get update
Get:1 https://download.docker.com/linux/ubuntu jammy InRelease [48.9 kB]
Get:2 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages [22.2 kB]
Hit:3 http://jp.archive.ubuntu.com/ubuntu jammy InRelease
Hit:4 http://jp.archive.ubuntu.com/ubuntu jammy-updates InRelease
Hit:5 http://jp.archive.ubuntu.com/ubuntu jammy-backports InRelease
Hit:6 http://security.ubuntu.com/ubuntu jammy-security InRelease
Fetched 71.0 kB in 1s (67.2 kB/s)
Reading package lists... Done
junenu@randall:~$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  bridge-utils systemd-hwe-hwdb ubuntu-fan
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  docker-buildx-plugin docker-ce-rootless-extras libslirp0 slirp4netns
Suggested packages:
  aufs-tools cgroupfs-mount | cgroup-lite
The following packages will be REMOVED:
  containerd docker.io runc
The following NEW packages will be installed:
  containerd.io docker-buildx-plugin docker-ce docker-ce-cli docker-ce-rootless-extras docker-compose-plugin libslirp0
  slirp4netns
0 upgraded, 8 newly installed, 3 to remove and 417 not upgraded.
Need to get 114 MB of archives.
After this operation, 143 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 https://download.docker.com/linux/ubuntu jammy/stable amd64 containerd.io amd64 1.6.24-1 [28.6 MB]
Get:2 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-buildx-plugin amd64 0.11.2-1~ubuntu.22.04~jammy [28.2 MB]
Get:3 http://jp.archive.ubuntu.com/ubuntu jammy/main amd64 libslirp0 amd64 4.6.1-1build1 [61.5 kB]
Get:4 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-ce-cli amd64 5:24.0.6-1~ubuntu.22.04~jammy [13.3 MB]
Get:5 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-ce amd64 5:24.0.6-1~ubuntu.22.04~jammy [22.6 MB]
Get:6 http://jp.archive.ubuntu.com/ubuntu jammy/universe amd64 slirp4netns amd64 1.0.1-2 [28.2 kB]
Get:7 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-ce-rootless-extras amd64 5:24.0.6-1~ubuntu.22.04~jammy [9,032 kB]
Get:8 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-compose-plugin amd64 2.21.0-1~ubuntu.22.04~jammy [11.9 MB]
Fetched 114 MB in 2s (47.5 MB/s)
(Reading database ... 203447 files and directories currently installed.)
Removing docker.io (24.0.5-0ubuntu1~22.04.1) ...
'/usr/share/docker.io/contrib/nuke-graph-directory.sh' -> '/var/lib/docker/nuke-graph-directory.sh'
Warning: Stopping docker.service, but it can still be activated by:
  docker.socket
Removing containerd (1.7.2-0ubuntu1~22.04.1) ...
Removing runc (1.1.7-0ubuntu1~22.04.1) ...
Selecting previously unselected package containerd.io.
(Reading database ... 203175 files and directories currently installed.)
Preparing to unpack .../0-containerd.io_1.6.24-1_amd64.deb ...
Unpacking containerd.io (1.6.24-1) ...
Selecting previously unselected package docker-buildx-plugin.
Preparing to unpack .../1-docker-buildx-plugin_0.11.2-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-buildx-plugin (0.11.2-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-ce-cli.
Preparing to unpack .../2-docker-ce-cli_5%3a24.0.6-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-ce-cli (5:24.0.6-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-ce.
Preparing to unpack .../3-docker-ce_5%3a24.0.6-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-ce (5:24.0.6-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-ce-rootless-extras.
Preparing to unpack .../4-docker-ce-rootless-extras_5%3a24.0.6-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-ce-rootless-extras (5:24.0.6-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-compose-plugin.
Preparing to unpack .../5-docker-compose-plugin_2.21.0-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-compose-plugin (2.21.0-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package libslirp0:amd64.
Preparing to unpack .../6-libslirp0_4.6.1-1build1_amd64.deb ...
Unpacking libslirp0:amd64 (4.6.1-1build1) ...
Selecting previously unselected package slirp4netns.
Preparing to unpack .../7-slirp4netns_1.0.1-2_amd64.deb ...
Unpacking slirp4netns (1.0.1-2) ...
Setting up docker-buildx-plugin (0.11.2-1~ubuntu.22.04~jammy) ...
Setting up containerd.io (1.6.24-1) ...
Setting up docker-compose-plugin (2.21.0-1~ubuntu.22.04~jammy) ...
Setting up docker-ce-cli (5:24.0.6-1~ubuntu.22.04~jammy) ...
Setting up libslirp0:amd64 (4.6.1-1build1) ...
Setting up docker-ce-rootless-extras (5:24.0.6-1~ubuntu.22.04~jammy) ...
Setting up slirp4netns (1.0.1-2) ...
Setting up docker-ce (5:24.0.6-1~ubuntu.22.04~jammy) ...
Could not execute systemctl:  at /usr/bin/deb-systemd-invoke line 142.
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
junenu@randall:~$ sudo docker -v
Docker version 24.0.6, build ed223bc
junenu@randall:~$ sudo docker compose version
docker: 'compose' is not a docker command.
See 'docker --help'
junenu@randall:~$ sudo docker compose
Display all 142 possibilities? (y or n)
junenu@randall:~$ sudo docker compose -v
Docker version 24.0.6, build ed223bc
junenu@randall:~$ docker compose version
Docker Compose version v2.4.1
junenu@randall:~$ getent group | grep docker
docker:x:136:
junenu@randall:~$ sudo usermod -aG docker $USER
junenu@randall:~$ getent group | grep docker
docker:x:136:junenu
junenu@randall:~$ exit
logout
Connection to 192.168.10.158 closed.
❯ ssh 192.168.10.158
junenu@192.168.10.158's password:
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 6.2.0-33-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

396 updates can be applied immediately.
244 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Last login: Thu Sep 28 15:09:49 2023 from 192.168.10.157
junenu@randall:~$ git clone -b release https://github.com/netbox-community/netbox-docker.git
Cloning into 'netbox-docker'...
remote: Enumerating objects: 4358, done.
remote: Counting objects: 100% (211/211), done.
remote: Compressing objects: 100% (137/137), done.
remote: Total 4358 (delta 101), reused 148 (delta 68), pack-reused 4147
Receiving objects: 100% (4358/4358), 1.06 MiB | 23.61 MiB/s, done.
Resolving deltas: 100% (2507/2507), done.
junenu@randall:~$ cd netbox-docker/
junenu@randall:~/netbox-docker$ tee docker-compose.override.yml <<EOF
version: '3.4'
services:
  netbox:
    ports:
      - 8000:8080
EOF
version: '3.4'
services:
  netbox:
    ports:
      - 8000:8080
junenu@randall:~/netbox-docker$ docker compose pull
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
junenu@randall:~/netbox-docker$ docker compose pull
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
junenu@randall:~/netbox-docker$ do
do                             dockerd-rootless-setuptool.sh  done
docker/                        dockerd-rootless.sh            do-release-upgrade
docker-compose                 docker-proxy                   dosfsck
dockerd                        domainname                     dosfslabel
junenu@randall:~/netbox-docker$ sudo service docker stop
sudo service docker start
[sudo] password for junenu:
junenu@randall:~/netbox-docker$ docker compose pull
[+] Running 20/20
 ⠿ redis-cache Pulled                                                                                               4.3s
 ⠿ netbox-worker Pulled                                                                                            18.3s
   ⠿ 10fb01f4f619 Already exists                                                                                    0.0s
   ⠿ 7690ee9b8a77 Pull complete                                                                                    12.1s
   ⠿ 9de79dbfae94 Pull complete                                                                                    12.5s
   ⠿ 00c635b6953a Pull complete                                                                                    12.7s
   ⠿ 5e93662d063f Pull complete                                                                                    12.8s
   ⠿ 4f4fb700ef54 Pull complete                                                                                    13.0s
   ⠿ f21b25c249a8 Pull complete                                                                                    13.8s
 ⠿ netbox Pulled                                                                                                   18.3s
   ⠿ 15a47b805cb6 Pull complete                                                                                    12.6s
   ⠿ b2fb818b23b0 Pull complete                                                                                    12.7s
   ⠿ dcf8e05ab747 Pull complete                                                                                    12.8s
 ⠿ postgres Pulled                                                                                                  4.3s
 ⠿ netbox-housekeeping Pulled                                                                                      18.3s
   ⠿ ccbb507ab45d Pull complete                                                                                     2.7s
   ⠿ 0662e9b27d65 Pull complete                                                                                    12.6s
   ⠿ 41821d4eb1de Pull complete                                                                                    12.9s
   ⠿ 6007dd4387cd Pull complete                                                                                    13.0s
 ⠿ redis Pulled                                                                                                     4.4s
junenu@randall:~/netbox-docker$ docker compose up -d
[+] Running 6/6
 ⠿ Container netbox-docker-postgres-1             Started                                                           1.2s
 ⠿ Container netbox-docker-redis-cache-1          Started                                                           1.4s
 ⠿ Container netbox-docker-redis-1                Started                                                           1.3s
 ⠿ Container netbox-docker-netbox-1               Healthy                                                          18.2s
 ⠿ Container netbox-docker-netbox-worker-1        Started                                                          18.6s
 ⠿ Container netbox-docker-netbox-housekeeping-1  Started                                                          18.7s
junenu@randall:~/netbox-docker$ sudo docker ps
CONTAINER ID   IMAGE                               COMMAND                  CREATED          STATUS                    PORTS                                       NAMES
ea840374ccf6   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   51 seconds ago   Up 33 seconds (healthy)                                               netbox-docker-netbox-housekeeping-1
6982f187d8f3   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   51 seconds ago   Up 33 seconds (healthy)                                               netbox-docker-netbox-worker-1
4f0be816faf3   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   52 seconds ago   Up 50 seconds (healthy)   0.0.0.0:8000->8080/tcp, :::8000->8080/tcp   netbox-docker-netbox-1
596d622f08c7   redis:7-alpine                      "docker-entrypoint.s…"   52 seconds ago   Up 50 seconds             6379/tcp                                    netbox-docker-redis-1
9e67aeb36217   redis:7-alpine                      "docker-entrypoint.s…"   52 seconds ago   Up 50 seconds             6379/tcp                                    netbox-docker-redis-cache-1
b712ede8a944   postgres:15-alpine                  "docker-entrypoint.s…"   52 seconds ago   Up 50 seconds             5432/tcp                                    netbox-docker-postgres-1
junenu@randall:~/netbox-docker$ sudo docker ps
CONTAINER ID   IMAGE                               COMMAND                  CREATED              STATUS                        PORTS                                       NAMES
ea840374ccf6   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   About a minute ago   Up About a minute (healthy)                                               netbox-docker-netbox-housekeeping-1
6982f187d8f3   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   About a minute ago   Up About a minute (healthy)                                               netbox-docker-netbox-worker-1
4f0be816faf3   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   About a minute ago   Up About a minute (healthy)   0.0.0.0:8000->8080/tcp, :::8000->8080/tcp   netbox-docker-netbox-1
596d622f08c7   redis:7-alpine                      "docker-entrypoint.s…"   About a minute ago   Up About a minute             6379/tcp                                    netbox-docker-redis-1
9e67aeb36217   redis:7-alpine                      "docker-entrypoint.s…"   About a minute ago   Up About a minute             6379/tcp                                    netbox-docker-redis-cache-1
b712ede8a944   postgres:15-alpine                  "docker-entrypoint.s…"   About a minute ago   Up About a minute             5432/tcp                                    netbox-docker-postgres-1
junenu@randall:~/netbox-docker$
junenu@randall:~/netbox-docker$
junenu@randall:~/netbox-docker$ exit
logout
Connection to 192.168.10.158 closed.
❯ ssh 192.168.11.101
^C
❯ ssh 192.168.11.111
The authenticity of host '192.168.11.111 (192.168.11.111)' can't be established.
ED25519 key fingerprint is SHA256:DdS6iT+WTp46KqZAgx1n9vDhbfh7KW2qErPQEH8aUUs.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:207: 192.168.10.158
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.11.111' (ED25519) to the list of known hosts.
junenu@192.168.11.111's password:
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 6.2.0-33-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

396 updates can be applied immediately.
244 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Last login: Thu Sep 28 15:29:07 2023 from 192.168.10.157
junenu@randall:~$ ls
Desktop  Documents  Downloads  Music  netbox-docker  Pictures  Public  snap  Templates  Videos
junenu@randall:~$ sudo docker ps
[sudo] password for junenu:
sudo: a password is required
junenu@randall:~$ docker ps
CONTAINER ID   IMAGE                               COMMAND                  CREATED         STATUS                   PORTS                                       NAMES
ea840374ccf6   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   6 minutes ago   Up 6 minutes (healthy)                                               netbox-docker-netbox-housekeeping-1
6982f187d8f3   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   6 minutes ago   Up 6 minutes (healthy)                                               netbox-docker-netbox-worker-1
4f0be816faf3   netboxcommunity/netbox:v3.6-2.7.0   "/usr/bin/tini -- /o…"   7 minutes ago   Up 6 minutes (healthy)   0.0.0.0:8000->8080/tcp, :::8000->8080/tcp   netbox-docker-netbox-1
596d622f08c7   redis:7-alpine                      "docker-entrypoint.s…"   7 minutes ago   Up 6 minutes             6379/tcp                                    netbox-docker-redis-1
9e67aeb36217   redis:7-alpine                      "docker-entrypoint.s…"   7 minutes ago   Up 6 minutes             6379/tcp                                    netbox-docker-redis-cache-1
b712ede8a944   postgres:15-alpine                  "docker-entrypoint.s…"   7 minutes ago   Up 6 minutes             5432/tcp                                    netbox-docker-postgres-1
junenu@randall:~$
```