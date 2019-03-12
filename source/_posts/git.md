---
title: Git 常用命令记录
date: 2019-03-12
description: Git的一些常用命令记录，目前仅用于本人开发时参考。
tags: 整理
---

### 暂存

```
暂存: git stash
撤销暂存: git stash pop
```



### 标签

```
新增标签:               git tag v12.2.0
推送到远程仓库(指定tag):  git push origin v12.2.0
推送到远程仓库(所有tag):  git push origin --tag
删除本地分支:            git tag -d v12.2.0
删除远程分支:            git push origin --delete tag v12.2.0
```
