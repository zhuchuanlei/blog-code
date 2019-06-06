---
title: parseInt 详解
date: 2019-02-24
keywords: [javaScript基础 parseInt]
description: 由题目['1','2','3'].map(parseInt)介绍 parseInt 的一些用法和规则。
top: 10
# tags: 语法
---

### 写在前面

先动动脑筋，做一下题目: `['1','2','3'].map(parseInt)`

我的思路: `map`会把每一项的元素、索引、数组传给`parseInt`，经过`parseInt`返回一个整数，最终得到一个新数组 `[1, 2, 3]`。
和你的答案一样吗？如果一样，那你要好好看看下面的文章了😂

### parseInt语法与定义

`用法` parseInt(string, radix); 具体语法参考w3school的[解释](http://www.w3school.com.cn/js/jsref_parseInt.asp)


### 吐槽一下

先看看官网给出的例子

``` 官网例子
parseInt("10");         //返回 10
parseInt("19",10);      //返回 19 (10+9)
parseInt("11",2);       //返回 3 (2+1)
parseInt("17",8);       //返回 15 (8+7)
parseInt("1f",16);      //返回 31 (16+15)
parseInt("010");        //未定：返回 10 或 8
```

当时我就想吐槽，既然给出了例子，敢不敢别这么有规律。比如可以列一下这些例子:

``` 例子
parseInt("11", 2);      //返回 3 (2+1)
parseInt("27",8);       //返回 23 (2*8+7)
parseInt(0x11);         //返回 17
```

### 解释

下面看一下两个参数的解释:

`参数string`

1. parseInt的第一个参数是`字符串`类型，当传入非字符串类型时，会先将其转换为字符串，如上例`parseInt(0x11)`；
2. 字符串转整数时，是一个一个字符依次转换的，遇到不能转为数字的字符，就终止转换，返回已经转换好的部分，如`parseInt('14a5')`；
3. 当 string 里前含有空格，会先去除前面的空格，如`parseInt('  122')`；
4. 放 string 里第一个字符不能转换为数字，则返回`NaN`，如`parseInt(' ')`。


`参数radix`
1. radix可以接受的范围是`2~36`，表示传入的字符串的进制，最终返回该值对应的十进制整数，如上例`parseInt("11", 2)`；
2. 当传入其他数字时返回`NaN`；
3. 默认是10，当传入 0、 undefined、null、false或''时，直接被忽略，如`parseInt("11", null)`。

再看几个例子

``` 例子
parseInt(010);          //返回 8 (parseInt('8'))
parseInt("14a5", 16);   //返回 5285 (1*16*^3+4*16^2+10*16^1+5)
parseInt('  12 2');     //返回 12 (parseInt('12'))
parseInt('21', 3);      //返回 7 (2*3*^1+1)
parseInt('23', 3);      //返回 2 (parseInt('2', 3))
parseInt('32', 3);      //返回 NaN (三进制数里没有3)
```

***

现在重新看一下上面的题目，完整的书写方式:
```
['1','2','3'].map(function(value, index, array){
    return parseInt(value, index, array);
})
```
分步执行一下:
```

parseInt('1', 0);        返回 1
parseInt('2', 1);        返回 NaN (参数radix 的第2条)
parseInt('3', 2);        返回 NaN (参数string 的第4条)
```

所以 `['1','2','3'].map(parseInt)` 的正确答案是 `[1, NaN, NaN]`。

### 收获

- 对于`parseInt`两个参数的规则，一定要清楚；
- 不管工作还是面试，不要粗心。