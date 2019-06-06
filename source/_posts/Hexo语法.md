---
title: Hexo 常用语法整理
date: 2019-01-03
keywords: [hexo markDown]
description: 本文介绍了 Hexo 的一些用法，如：代码块、图片、连接等，目前仅用于本人开发时参考。
tags: 整理
---

此文档是在已经建立好`Hexo`博客的前提

# 标题

``` 注释
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

# 图片

``` 
![](../image/test.png)
![](http://placekitten.com/g/20/20)
```

当找不到目标图片, 显示[]中的文字

![123](http://placekitten.com/g/20/20)


# 代码块
使用```
![代码块](../image/codeblock.png)

# 链接

```
[Hexo语法](https://zhuchuanlei.github.io/blog/Hexo语法/)
[Hexo语法]: https://zhuchuanlei.github.io/blog/Hexo语法/
```
[Hexo语法](https://zhuchuanlei.github.io/blog/Hexo语法/)
[Hexo语法]: https://zhuchuanlei.github.io/blog/Hexo语法/
```
# 分割线

```
***
---
___
```

***
---
___

# 表格

代码:

```
| name | age | sex |
|:----:|:---:|:---:|
| Peter | 18 | 男 |
| Petter | 19 | 男 |
| Tony | 19 | 男 |
```

样式:

| name | age | sex |
|:----:|:---:|:---:|
| Peter | 18 | 男 |
| Petter | 19 | 男 |
| Tony | 19 | 男 |

代码:

```
 name | age | sex 
 -|-|-
 Peter | 18 | 男 
 Petter | 19 | 男
 Tony | 19 | 男 
```

样式:

 name | age | sex 
 -|-|-
 Peter | 18 | 男 
 Petter | 19 | 男
 Tony | 19 | 男 
