---
title: sockjs-node 报错
date: 2019-06-06
keywords: [sockjs-node webpack]
description: 解决项目本地运行时一直报 sockjs-node/info 接口的错误
tags: []
---

### sockjs-node 报错

项目本地运行时一直报 sockjs-node/info 接口的错误，如下图:

![](../image/sockjs-node-error/1.png)

### 解决方法

1. 修改文件配置

- 找到/node_modules/sockjs-client/dist/sockjs.js，
- 找到代码的 1605行，
```js
try {
    // self.xhr.send(payload);
} catch (e) {
    self.emit('finish', 0, '');
    self._cleanup(false);
}
  ```
- 解决。

2. 修改webpack配置
```js
port = 8000;
{
    entry: [
        "webpack-dev-server/client?http://0.0.0.0:" + port,
    ],
    devServer: {
        port: port,
    }
}
```