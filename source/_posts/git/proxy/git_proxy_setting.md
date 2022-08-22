

# Git 设置代理和取消代理

## 如果代理类型是socks5进行如下设置即可

``shell
git config --global http.proxy socks5://127.0.0.1:1080 
git config --global https.proxy socks5://127.0.0.1:1080
```

## 如果是普通的http/https进行如下设置即可

```
git config --global https.proxy http://127.0.0.1:1080 
git config --global https.proxy https://127.0.0.1:1080
```

## 取消代理设置

```
git config --global --unset http.proxy 
git config --global --unset https.proxy
```