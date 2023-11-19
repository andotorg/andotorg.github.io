---
title: Git 基础知识记录分享 - reset hard
date: 2022-09-27
tags:
  - Git
  - reset
  - hard
categories: Git Base
---


# reset hard

## 更新工作目录

```shell
git reset --hard HEAD~ [filename.java]
```

> 该选项直接覆盖了工作目录，所以是直接删除了数据
> 只能铜鼓reflog进行恢复(文件提交的情况下)
