---
title: 面试题集锦
date: 2019-04-09
description: 记录一些面试题
tags: 整理
---

### JS

js相关

#### 数组去重
```js
var arr = [1,2,1,2,3,5,4,5,3,4,4,4,4];
// 第一种
[...new Set(arr)]
// 第二种
init=[]
var result = arr.sort().reduce((init, current)=>{               // sort 排序
    console.log(init,current)                                   // reduce(func, defaultValue) 是ECMAScript5规范中出现的数组方法 
    if(init.length===0 || init[init.length-1]!==current){       // 遍历每个元素，执行func, 
        init.push(current);
    }
    return init;
}, []);
console.log(result);   // [1, 2, 3, 4, 5]
```

### AJAX

```js
//创建 XMLHttpRequest 对象
var xml = new XMLHttpRequest();
//规定请求的类型、URL 以及是否异步处理请求。
xml.open('GET', url, true);     // 第三个参数true-异步，false-同步
//发送信息至服务器时内容编码类型
xml.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); 
//发送请求
xml.send(null);  
//接受服务器响应数据
xml.onreadystatechange = function () {
    if (obj.readyState == 4 && (obj.status == 200 || obj.status == 304)) {
        // code
    }
};
```

#### json对象转换json字符串

```js
//字符串转对象
JSON.parse(json)
eval('(' + jsonstr + ')')   
// 对象转字符串
JSON.stringify(json)
```

#### 跨域解决方案有哪些

1. jsonp 只能解决get跨域
```js
// 主页面

<body>
  <div id="app">1
  </div>
  <script>
    localHandler = function(data){
        console.log(data);                 // 打印结果: {code: 'CA1998', price: 1780, tickets: 5}
    };
    var url = "http://localhost:8002/jsonp.js?code=CA1998&callback=localHandler";
    var script = document.createElement('script');
    script.setAttribute('src', url);
    document.getElementsByTagName('head')[0].appendChild(script); 
  </script>
</body>

// jsonp.js
localHandler({
    "code": "CA1998",
    "price": 1780,
    "tickets": 5
});
```



DOM,BOM,AJAX,JSON,
深刻理解Web标准，对可用性、可访问性等相关知识
node


### CSS

css 相关

#### CSS盒模型

1. CSS盒模型是什么？
盒子模型包括：content、padding、margin、border

2. 标准模型和IE模型的区别？
    * 标准模型的宽高为content的宽高
    * IE模型的宽高包括border

3. CSS如何设置这两种模型？
    * 标准模型：box-sizing：content-box
    * IE模型：box-sizing：border-box

`标准盒模型`和`IE怪异盒模型`

`box-sizing`
border-box: width = contentWidth + borderWidth + paddingWidth;
content-box: width = contentWidth;

#### BFC

1. BFC是什么，讲一下原理
块级格式化上下文


#### 让一个元素水平垂直居中

### Promise

Promise 相关

```js
p1 = new Promise(fn);       // Promise {<pending>}
p2 = Promise.resolve(p1)    // Promise {<pending>}
p1 === p2;                  // true
```
```js
p1 = Promise.resolve(123); 
//打印p1 可以看到p1是一个状态置为resolved的Promise对象
console.log(p1);              // Promise { <resolve> : 123}
p1.then((value)=>{
    console.log(value);       // 123
});
```
```js
console.log(1)
p = Promise.resolve(setTimeout(console.log(2), 100))
p.then(()=>{
    console.log(3)
})
console.log(4);
输出结果: 1 2 4 3
```

```js 
function buy(resolve, reject) {
    console.log(1);
    setTimeout(function(){
        console.log(2);
        resolve(['西红柿', '鸡蛋', '油菜']);              // 打印结果
    },3000)                                             // 1        // 等待3s
}                                                       // 2
function cook(resolve, reject){                         // ['西红柿', '鸡蛋', '油菜']
    console.log(3);                                     // 3        // 等待3s
    setTimeout(function(){                              // 4
        console.log(4);                                 // 饭
        resolve ('饭')                                  // 5
    },3000)                                             // 送达
}                                                       // 6
function send(resolve, reject){
    console.log(5);
    resolve('送达');
}
function callMe(){
    console.log(6);
}

new Promise(buy)
.then((value)=>{
    console.log(value);
    return new Promise(cook)
})
.then((value)=>{
    console.log(value);
    return new Promise(send)
})
.then((value)=>{
    console.log(value);
    callMe()
})
```

面试题
```js
const first = () => (new Promise((resolve,reject)=>{
    console.log(3);
    let p = new Promise((resolve, reject)=>{
         console.log(7);
        setTimeout(()=>{
           console.log(5);
           resolve(6); 
        },0)
        resolve(1);
    }); 
    resolve(2);
    p.then((arg)=>{
        console.log(arg);
    });

}));

first().then((arg)=>{
    console.log(arg);
});
console.log(4);
```
打印结果: `3 7 4 1 2 5`
`第一轮事件循环`:
先执行`宏任务`，主Script，new Promise 立即执行，输出`3`，执行p的 new Promise，输出`7`，发现setTimeout，放进任务队列里，p的then(then1)，放进`微任务队列`，发现first的then(then2)，放进`微任务队列`，输出`4`，宏任务执行完毕。
执行`微任务`，执行then1，输出`1`，执行then2，输出`2`；
`第二轮事件循环`:
先执行`宏任务`，即执行setTimeout的回调，输出`5`，下面一行的resolve不会生效，因为输出1时已经执行了，状态已经改变。
* setTimeout的回调比Promise.then的优先级低

[更多Promise的题目](https://juejin.im/post/5bd697cfe51d454c791cd1d5)

### async await

async await 相关

### React

react相关

#### react 通信

父传子: `Props`
子传父: `Props` + `callback`
父传子的子
```js
    1. `Props`
    2. `Context`
        父
            static childContextTypes = {color: PropTypes.string}
            getChildContext(){ return color }
        子的子
            static contextTypes = {color: PropTypes.string}
            this.context.color
```
非嵌套组件: 订阅

#### React如何使用 Virtual Dom 及其优势

在JS中操作`Dom`的开销由于会引发浏览器的`重排重绘机制`, 性能消耗十分显著, 然后在JS中操作对象的效率很高。React团队通过`Diff`算法, 对比新旧两个 `VDom` 的内容, 找出不同, 一次性更新`Dom`, 减少开销。React团队通过直接对比相同层的内容等算法的优化, 将算法复杂度降低到`O(n)`。

#### setState 执行后为什么state无法正确获取设置之后的值

`setState`之后, `React`并没有立刻更改 `state` 的值, 因为每次改变`state`都去计算生成新的`VDom`树并更新`Dom`是一件代价高昂的事情, `React`的做法是将`setState`设计成为一个异步行为, 多次`setState`会合并成为一次, 在最后去计算生成`Dom`, 优化性能。
```
this.setState({
    a: 5
}, () => {
    // setState 提供的第二个参数是一个回调函数
    // 这里能够正确获取 state 里面的值
})
```

#### 为什么建议使用 Stateless Function

1. 代码更少, 逻辑更清晰
2. 更加符合 React 数据流从顶部开始的设计初衷
3. 不用使用 bind 方法

### ES6

es6相关

#### let const的命令

```js
//变量提升
console.log(a);  // 1
console.log(b);  // 爆炸
console.log(c);  // 爆炸
var a = 1;
let b = 2;
const c = 3;

// 全局声明
console.log(window.a)  //  1 

// 重复声明
let b  = 200;//爆炸
```
`let`和`const`都不能变量提升的，如果在变量声明前调用会产生暂缓性死区，报错。
它们声明后也不会绑在`window`（浏览器）和`global`（node）上，而`var`可以。
它们不能重复声明，并且`const`为常量，不能修改值。

|\|变量提升|重复声明|全局声明|
|-|-|-|-|
|var|yes|yes|yes|
|let|no|no|no|
|const|no|no|no|

#### Array

array.map
array.filter
...array
let newArray = Array.from(...new Set(array))

#### Object

```js
Object.assign(a, b)  // b的每个元素merge到a里，返回a 
Object.keys();       // 返回数组，内容是a的所有key
Object.values()      // 返回数组，内容是a的所有值
Object.entries()     // 返回数组，内容是a的key,value组成的数组，[[key1, value1], [key2, value2]]
```

### Vue

Vue 相关

#### v-if 与 v-show 的区别

`v-if` 会将 Dom 直接从当前Dom树移除;
`v-show` 则是相当于设置了 `display: none`, 但是 Dom 依旧存在于Dom树中
两种情况各有利弊，类似缓存，视情况不同选择不同。

#### computed 与 methods 的区别

`computed` 方法会有缓存值,
`methods` 不会存在缓存值。

### webpack

`webpack`是一个模块打包工具，可以使用它管理项目中的模块依赖，并编译输出模块所需的静态文件。它可以很好地管理、打包开发中所用到的`HTML`,`CSS`,`JavaScript`和静态文件（图片，字体）等，让开发更高效。对于不同类型的依赖，`webpack`有对应的模块加载器，而且会分析模块间的依赖关系，最后合并生成`优化`的`静态资源`。
[地址](https://juejin.im/post/5c6cffde6fb9a049d975c8c1)

可以用一些官方脚手架

### 其他

#### Cookie, LocalStorage 与 SessionStorage
|特性|Cookie|localStorage|SessionStorage|
|-|-|-|-|
|数据的生命周期|可设置失效时间，默认是关闭浏览器后失效|除非被清除，否则永远存在|仅在当前会话下有效，关闭页面或浏览器后被清除|
|存放数据大小|4K左右|一般为5MB|一般为5MB|
|与服务器端通信|每次都会携带在HTTP头中，如果保存过多数据会带来性能问题|仅在客户端中保存，不参与和服务器通信|仅在客户端中保存，不参与和服务器通信|
|易用性|需要自己封装，原生的Cookie接口不友好|原生接口可以接受，也可以再封装来对Object和Array|原生接口可以接受，也可以再封装来对Object和Array|

