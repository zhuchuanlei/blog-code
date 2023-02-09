---
title: 贝塞尔曲线及其应用
date: 2021-12-30
keywords: [CSS 贝塞尔曲线]
description: 贝塞尔曲线不仅CSS中用到了，在JS中也同样可以用到
tags: [js 整理]
---


### 简介

在前端工作中，`贝塞尔曲线`被多个地方应用。就如最近年会抽奖中，开始滚动的加速度和结束滚动时如何缓慢停止在中奖号码的位置，就用到了`贝塞尔曲线`。所以顺势就介绍一下`贝塞尔曲线`和简单应用。
其一般参数公式：   

![](../image/bezier/normal.svg)

该曲线是由`P0`开始，止于`Pn`，中间有`n-2`个点控制曲线的走势。根据控制点个数不同，得到不同的特殊曲线公式。
<!-- 下面是3个常见的曲线公式 -->

<!--  -->
在实际工作中，常用的是`二次贝塞尔曲线`和`三次贝塞尔曲线`。一次贝塞尔曲线，呈现出来的是一条直线。下面是几种常见的贝塞尔曲线的介绍和应用。

### 常见的曲线

#### 线性公式  
`n=1`时，控制点个数: `0`，仅有开始和结束两个点，得到的是一条直线。  
![](../image/bezier/bezier1.svg)  

#### 二次方公式   
`n=2`时，控制点个数: `1`  
![](../image/bezier/bezier2.svg)  

#### 三次方公式    
`n=3`时，控制点个数: `2`  
![](../image/bezier/bezier3.svg)   


#### 公式说明  
以`二次方公式`为例，转换为函数如下，其中P0为起点，P2为终点，P1为控制点。得到的是从P0到P2的`点`关于`t`的二次函数，t的范围[0, 1]。  
```js
function QuadraticBezier(P0, P1, P2){
  return (t) => (1 - t) * (1 - t) * P0 + 2 * t * (1 - t) * P1 + t * t * P2;
}
```

### 应用

#### SVG的应用

在`SVG`中，用`q`或者`Q`可以绘制`二次方贝塞尔曲线`(q指相对位置，Q指绝对位置)。以[svg](https://www.runoob.com/try/try.php?filename=trysvg_path2)为例，
关键代码`<path d="M 100 350 Q 250 50 400 350"/> `，起点P0`(100 350)`, 终点P2`(400 350)`, 控制点P1`(250 50)`。将X坐标和Y坐标分别带入函数`QuadraticBezier`中，可以获得某个时刻的具体坐标。

```js
// 分别计算X、Y
const getX = new QuadraticBezier(100, 250, 400);
const getY = new QuadraticBezier(350, 50, 350);

const point = document.getElementById("point");  // 随着t动态移动的点
let t = 0;  // 初始值为0，逐步增加到1
function setXY () {
  point.style.top = getY(t) + "px";
  point.style.left = getX(t) + "px";
  if (t >= 1) {
    cancelAnimationFrame(frame);
  } else {
    t = t + 0.005;
    frame = requestAnimationFrame(setXY);
  }
}
let frame = requestAnimationFrame(setXY);
```
随着`t`的增加，分别计算出`point`的top和left，可以得到如下动画：
<iframe src="../image/bezier/svg.mp4" width="300" height="300"></iframe>  

`canvas`中的api`quadraticCurveTo`、`bezierCurveTo`与绘制`SVG`类似。



#### CSS animation-timing-function

在`CSS`动画中由`animation-timing-function`规定动画的速度曲线，已经预设了几个值：
`linear`、`ease`、`ease-in`、`ease-out`、`ease-in-out`，自定义速度时使用`cubic-bezier()`灵活控制。
可以修改[例子](https://www.runoob.com/try/try.php?filename=trycss3_animation-timing-function3)中数字看看效果。

#### JS中应用

CSS属性`animation-timing-function`的值`cubic-bezier`接收的值正常范围是[0, 1]的，即在`三次贝塞尔曲线`中，默认起始点`P0(0, 0)`，终点`P3(1, 1)`，可以推导出公式
```js
const CubicBezier = P0 * (1-t)^3 + 3 * P1 * t * (1-t)^2 + 3 * P2 * t^2 * (1-t) + P3 * t^3
// 令 P0(0, 0), P3(1, 1), P1(x1, y1), P2(x2, y2)，代入
x = 3 * x1 * t * (1-t)^2 + 3 * x2 * t^2 * (1-t) + t^3;
  = (3 * x1 - 3 * x2 + 1) * t^3 + (3 * x2 - 6 * x1) * t^2 + 3 * x1 * t;
y = (3 * y1 - 3 * y2 + 1) * t^3 + (3 * y2 - 6 * x1) * t^2 + 3 * y1 * t;

即
function CubicBezier(x1, y1, x2, y2) {
  this.x1 = x1;
  this.y1 = y1;
  this.x2 = x2;
  this.y2 = y2;
}
CubicBezier.prototype.sampleCurveX = function(t){
  const x1 = this.x1, x2 = this.x2;
  return (3 * x1 - 3 * x2 + 1) * t^3 + (3 * x2 - 6 * x1) * t^2 + 3 * x1 * t;
}
CubicBezier.prototype.sampleCurveY = function(t){
  const y1 = this.y1, y2 = this.y2;
  return (3 * y1 - 3 * y2 + 1) * t^3 + (3 * y2 - 6 * x1) * t^2 + 3 * y1 * t;
}

```
将`X`和`Y`的函数结合起来，即可计算t时刻的值：
```js
CubicBezier.prototype.solve = function(t){
  return this.sampleCurveY(this.sampleCurveX(t))
}
```
此时简单结合起来，误差较大。因为根据绘制SVG例子中可以看出，函数`sampleCurveX`和`sampleCurveY`计算的是`t`时刻的坐标`x`和`y`，能画出该`三次贝塞尔曲线`。但是应用到属性值的变化上时，曲线上`t`时刻的`切线`代表`t`时刻属性值变化的`速度`，与`t`时刻的坐标`x`和`y`不是直接关系。所以需要在对函数进行再加工。牛顿迭代法：


```js
CubicBezier.prototype.solve = function(x){
  if (x === 0 || x === 1) {             // 对 0 和 1 两个特殊 t 不做计算
    return this.sampleCurveY(x);
  } 
  let t = x
  for (let i = 0; i < 8; i++) {         // 进行 8 次迭代
    const g = this.sampleCurveX(t) - x
    if (Math.abs(g) < this.epsilon) {   // 检测误差到可以接受的范围, 如：this.epsilon = 1e-7;
      return this.sampleCurveY(t)
    }
    const d = 3 * (3 * x1 - 3 * x2 + 1) * t^2 + 2 * (3 * x2 - 6 * x1) * t + 3 * x1;  // 对 x 求导
    if (Math.abs(d) < 1e-6) {           // 如果梯度过低，说明牛顿迭代法无法达到更高精度
      break
    }
    t = t - g / d;
  }
  return this.sampleCurveY(t)                   // 对得到的近似 t 求 y
}
``` 
在误差可接受的范围内，迭代后的效果与CSS动画还是有微微的差距的。测试效果如下:
<iframe src="image/bezier/iteration.mp4" width="100" height="300"></iframe>  

#### 老虎机中的应用
在年会抽奖页面中，也用到了`三次贝塞尔曲线`，如：开始滚动时，滚动速度要从0加速到指定速度`maxSpeed`，需要传入：初始速度0，最大速度maxSpeed，当前时间，加速到maxSpeed所需时间以及控制点。可得如下函数：
```js
function initCubicBezier(startTime, totalTime, startValue, targetValue, controlArr){
  let cubic = new CubicBezier(...controlArr);
  function getValue(time){
    let progress = (time - startTime) / totalTime;
    if (progress >= 1) {
      progress = 1
    }
    const value = cubic.solve(progress);
    return value * (targetValue - startValue) + startValue;
  }
  return getValue;
}
```
1. 开始滚动

    1. 创建获取当前时间对应速度的函数`getSpeed`
    2. 根据`top`值使图片偏移，达到移动效果
    3. `top = top + speed`, 修改`speed`的值使增量变化，达到加速效果
    4. 速度未达到最大速度时，`getSpeed`获取新的速度
    5. 重复2-4，主要代码如下

```js
function start() {
  let top = 0; // 当前图片移动的位置
  let speed = 0;  // 初始速度
  const targetSpeed = 100;  // 最大速度
  const totalTime = 3000;   // 3000毫秒加速到最大速度
  const getSpeed = initCubicBezier(date.now(), totalTime, speed, targetSpeed, [.5,.5,.5,.5]);
  
  function run () {
    if (speed < targetSpeed) {
      speed = getSpeed(Date.now());
    }
    top = top + speed;
    drawImage(top);  // 重新绘制图片
    requestAnimationFrame(run);
  }
  requestAnimationFrame(run); 
}
```

2. 结束滚动

    1. 需要结束时根据`top`计算当前显示的数字`showNum`
    2. 根据中奖数字`targetNum`和`showNum`计算出，停止滚动时的值`targetTop`
    3. 创建获取某时刻的top函数`getTop`
    4. 计算此刻图片的偏移量`top`
    5. 判断`top < targetTop`时，说明还未滚动中奖号码，继续滚动
    6. 判断`top >= targetTop`时，停止滚动。


```js
function end(targetNum) {
  const targetTop = getTargetTop(top, targetNum);  // 根据当先位置，和中奖数字，获取停留的位置
  const getTop = initCubicBezier(date.now(), 3000, top, targetTop, [.39,.61,.74,.99]);
  function run () {
    if (top >= targetTop) {
      cancelAnimationFrame(frame);
    } else {
      top = getTop(Date.now());
      drawImage(top);  // 绘制新图片
      frame = requestAnimationFrame(run);
    }
  }
  let frame = requestAnimationFrame(run);
}
```

最终效果如下:

<iframe src="../image/bezier/number.mp4" width="200" height="200"></iframe>  