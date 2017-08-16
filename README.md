# Docker ARM32v6 Samba Server

## Automated Build Status
[![CircleCI](https://circleci.com/gh/fabianotessarolo/docker-arm32v6-samba-server.svg?style=svg)](https://circleci.com/gh/fabianotessarolo/docker-arm32v6-samba-server)

This is a simple Samba Docker Container based on hypriot/rpi-alpine:3.6 (Based on arm32v6/alpine:3.6) image.

By default, the share will be accessible read-only for everyone, with write access for user "rio" with password "letsdance". See smb.conf for details, or feel free to use your own config (see below).

Runs Samba's smbd and nmbd within the same container, using supervisord. Due to the fact that nmbd wants to broadcast
and become the "local master" on your subnet, you need to supply the "--net=host" flag to make the server visible to the hosts subnet (likely your LAN).

Quick start for the impatient:
```shell
docker run -d --net=host -v /path/to/share/:/shared --name samba pwntr/samba-alpine
```

When NetBIOS discovery is not needed
```shell
docker run -d -p 137-139:137-139 -p 445:445 -v /path/to/share/:/shared --name samba pwntr/samba-alpine
```

With your own smb.conf and supervisord.conf configs:
```shell
docker run -d -p 137-139:137-139 -p 445:445 -v /path/to/configs/:/config -v /path/to/share/:/shared --name samba pwntr/samba-alpine
```

To have the container start when the host boots, add docker's restart policy:
```shell
docker run -d --restart=always -p 137-139:137-139 -p 445:445 -v /path/to/share/:/shared --name samba pwntr/samba-alpine
```
