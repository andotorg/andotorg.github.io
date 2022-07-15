---
title: CentOS 设置开机启动Redis
date: 2022-07-02 15:11:00
tags: Java,CentOS,Spring,Linux,Redis
---

# CentOS 设置开机启动Redis

## 创建相关用户，并设置权限

```shell
sudo gropdadd redis
sudo useradd -g redis redis --no-create-home
sudo chown -R redis:redis /usr/local/redis
```

> 用户如果创建成功

## 创建文件，并书写脚本

```shell
vim /etc/systemd/system/redis-server.service

[Unit]
Description=Redis Server Manager
After=syslog.target
After=network.target

[Service]
Type=simple
User=redis
Group=redis
PIDFile=/var/run/redis_6379.pid
ExecStart=/usr/local/redis-5.0.14/src/redis-server /usr/local/redis-5.0.14/redis.conf
ExecStop=/usr/local/redis-5.0.14/src/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

> 如果用户没有创建成功

## root用户启动脚本

```shell
vim /etc/systemd/system/redis-server.service

[Unit]
Description=Redis Server Manager
After=syslog.target
After=network.target

[Service]
Type=simple
User=root
Group=root
PIDFile=/var/run/redis_6379.pid
ExecStart=/usr/local/redis-5.0.14/src/redis-server /usr/local/redis-5.0.14/redis.conf
ExecStop=/usr/local/redis-5.0.14/src/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

## 设置开机启动

```shell
systemctl daemon-reload 
systemctl start redis-server.service 
systemctl enable redis-server.service
```

## 创建redis-cli软连接

```shell
ln -s /usr/local/redis/redis-cli /usr/bin/redis-cli
```
