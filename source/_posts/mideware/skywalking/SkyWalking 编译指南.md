---
title: SkyWalking 编译指南
date: 2022-07-12 15:11:00
tags: Java,SkyWalking,Spring,Linux,WuSheng
---

# SkyWalking 编译指南

## 源码下载

clone git 上的SkyWalking源码

https://github.com/apache/skywalking

```shell
git clone git@github.com:apache/skywalking.git
```



## GRPC协议项目源码下载

进入目录，clone grpc 协议源码

```shell
git clone git@github.com:apache/skywalking-data-collect-protocol.git
```



> 关于该问题的相关issues：https://github.com/apache/skywalking/issues/4864

```text
这个问题是因为SkyWalking项目中的 network模块中的proto里面的文件都单独存放到另一个项目中了，单独clone 或者下载SkyWalking的源码是不可以的，需要把对应 apache/skywalking-data-collect-protocol 项目clone或者下载到 skywalking/apm-protocol/apm-network/src/main/proto 目录中，就可以了
```

前端源码下载

```shell
cd skywalking/skywalking-ui
git clone git@github.com:apache/skywalking-rocketbot-ui.git
```

> 下载到对应目录下面， 如果不下载会出现 编译过程中会出现无法正常执行npm 等编译并发布前端的问题

## 编译

编译命令

```she
mvn clean compile -Dmaven.test.skip=true
```



编译时一个  long long time 的事情，一直等待..

>  如果可以更换下网络可以快一些

编译成功会显示

```she
```

编译成功后这个GRPC文件显示这样的icon

![image-20220521224645836](/Users/lucas/Documents/skywalking-docs/img/image-20220521224645836.png)

执行Maven命令打包

```shel
mvn clean package -Pagent //只处理 javaAgent部分，这在调试Agent的时候就减少许多时间
mvn package -Pbackend,dist//只处理OapServer并打包压缩
mvn package -Pui,dist//只处理UI并打包压缩
mvn package -Pagent,dist//只处理 javaAgent 并打包压缩
```





