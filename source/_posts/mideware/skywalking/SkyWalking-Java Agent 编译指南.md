---
title: SkyWalking-Java Agent 编译指南
date: 2022-07-14 15:11:00
tags: Java,SkyWalking,Spring,Linux,WuSheng,Agent,Monitor
---

# SkyWalking-Java Agent 编译指南

1. 第一步下载源码（通过git clone方式）

   ```shel
   git clone git@github.com:apache/skywalking-java.git
   cd skywalking-java
   git submodule init
   git submodule update
   ```

   或者直接传递```--recurse-submodules```参数

   ```shell
   git clone --recurse-submodules git@github.com:apache/skywalking-java.git
   ```

   > 没有了解过```git submodule```点击这里查看 [查看](http://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

2. 在 IntelliJ IDEA Terminal 中，进入skywalking-java目录

3. Maven进行编译

   ```shell
   mvn clean compile -Dmaven.test.skip=true
   ```

   

4. 设置 gRPC 的**自动生成**的代码目录，为**源码**目录 ：

   - /apm-network/target/generated-sources/protobuf/ 下的 `grpc-java` 和 `java` 目录

![image-20220521231409950](/images/sw/1.png)

![image-20220521231531897](/images/sw/image-20220521231531897.png)

最新版本的skywalking-java 要配合[apache](https://github.com/apache)/**[skywalking-data-collect-protocol](https://github.com/apache/skywalking-data-collect-protocol)** 至少是9.0.0才可以。

打包

```she
mvn package -Dmaven.test.skip=true
```

![image-20220522002708218](/images/sw/image-20220522002708218.png)

## 在IDEA启动一个SpringBoot Web项目

### 增加JVM参数

```
-javaagent:/Users/lucas/tal/github/skywalking-java/skywalking-agent/skywalking-agent.jar
-Dskywalking.agent.service_name=app-specs
-Dskywalking.collector.backend_service=192.168.1.9:11800
```

![image-20220522002859192](/images/sw/image-20220522002859192.png)