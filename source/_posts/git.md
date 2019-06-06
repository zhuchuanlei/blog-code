---
title: Git 常用命令记录
date: 2019-03-12
keywords: [git zsh oh-my-zsh]
description: Git的一些常用命令记录，目前仅用于本人开发时参考。
tags: 整理
---

### 暂存

```
git stash         // 暂存
git stash pop     // 撤销暂存
```

### 标签

```
git tag v12.2.0                           // 新增标签
git push origin v12.2.0                   // 推送到远程仓库(指定tag)
git push origin --tag                     // 推送到远程仓库(所有tag)
git tag -d v12.2.0                        // 删除本地分支
git push origin --delete tag v12.2.0      // 删除远程分支
```

### 代码撤回

```
撤销某次提交
git revert HEAD               // 撤回最近一次提交
git revert HEAD~n             // 撤回最近n次提交
git reset commit_id           // 撤回某次已经提交的代码

代码回滚到
git reset --hard commit_id    // 将代码会滚到某次提交代码中
git revert commit_id          // 将代码会滚到某次提交代码之前
```

### oh-my-zsh 常用命令

| zsh | git | 说明 |
| - | - | - |
| ga | git add |  |
| gb | git branch | |
| gbd | git branch -d | |
| gco | gti checkout | |
| gcb | git checkout -b | `gcb A B`从B分支切出分支A |
| gb | git diff |
| gm | git merge |
| gp | git push |
| gst | git status |
| gsta | git stash save |
| gstp | git stash pop |
| gcmsg | git commit -m |