---
title: MySQL知识分享 - DDL
date: 2022-11-01
tags:
  - mysql
  - ddl
categories: MySQL
---


# DDL

> 如果没有类型之间的使用差异，则默认使用carchar代替类型  
> 字符编码一律用utf8mb4  排序字符集一律用 utf8mb4_general_ci  

## 1. change field name

```sql
ALTER TABLE xxxxx
CHANGE COLUMN `xxx` `xxx_xx` varchar(255) 
CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '备注' AFTER `xxxx`,
```

`ALTER`
