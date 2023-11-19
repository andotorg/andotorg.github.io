---
title: Git 基础知识记录分享 - reset mixed
date: 2022-09-27
tags:
  - Git
  - reset
  - mixed
categories: Git Base
---


# reset mixed

## 更新索引

> --mixed 是默认选项

```shell
git reset --mixed HEAD~ [filename.java]

git reset HEAD~ [filename.java]
```

> 把head的文件复制到index中
