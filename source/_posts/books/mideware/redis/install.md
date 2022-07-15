# CentOS 安装Redis

## 首先下载redis源码，并使用tar进行解压缩

```shell
wget http://download.redis.io/releases/redis-5.0.14.tar.gz
tar xvzf redis-5.0.14.tar.gz
```

## 使用gcc进行编译

```shell
make
make test
```

## 编译后的可执行文件在/usr/local/redis-5.0.14/src目录下
