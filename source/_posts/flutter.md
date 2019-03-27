---
title: Flutter笔记
date: 2019-03-27
description: Flutter基础知识记录
tags: 整理
---

### 主题
方法 | 功能 | 例子
-|-|-
Theme.of(context) | 使用已经定义好的主题 | Theme.of(context).textTheme.title
copyWith | 拓展父主题，不覆盖父主题的情况下，设置某元素的主题 | Theme.of(context).copyWith(accentColor: Colors.blue)

### Dart语言

#### 变量和基本数据类型

Dart语言里一切皆为对象，所以如果没有将变量初始化，那么它的默认值为`null`。
```
int name;
name == null;     // true
```

1. 常量和固定值
`final`和`const`: 定义的是常量，不会变化，如果修改则会报错。

2. 基本数据类型
包括: `Number`、`String`、`Boolean`、`List`、`Map`。

+ String
```
'''内的字符串的格式会保留

var s1 = '''
按此格式输出
成都
'''
```

+ Boolean
Dart是强类型检查，只有bool类型的值是`true`才被认为是`true`。
```
var sex = '男';
if (sex) {                      // 报错 if (sex == 'name') 不报错
    print('你的性别是:' + sex);
}
```

+ List
Dart中的`List`对象类似于JS里的`Array`对象
```
    var list = [1, 2, 3, 4];
    print(list.length);       // 4
    print(list[0]);           // 1
```

+ Map
```
var week = {
    'Monday': '星期一'
}
或者
var week = new Map();
week['Monday'] = '星期一';

`assert(week['Monday'] == null);`   // 检查 key 是否在Map对象中
// assert 里为假值时 则报错
print(week.length);                 // week 的 key数量
不能使用 week.Monday
```

#### 函数

1. 可选参数: 参数默认必填，使用[]括起来表示可选 - `String getInfo(String name, [String form]){}`
2. 参数默认值: 只有可选参数才有默认值 - `String getInfo(String name, [String form = '中国']){}`
3. 所有函数必须有返回值，上面两个返回值必须是`String`类型，否则报错
4. 没有指定返回值时，默认返回值是`null`
5. 没有返回值的函数，系统会在最后添加隐式的`return`语句。


#### 运算符

```
/   除法
~/  取整
%   取余
== 等于

b ??= 2;   // b 为null时才把2赋值为b, 否则b仍为原定义的值;

a = a op b;  // ?????

```
1. 位运算
```
var value = 0x22;
var b = 0x0f;

&     // 与 a & b == 0x22, 与非 a & -b == 0x20;
|     // 或 a | b = 0x0f;
^     // 异或 a ^ b == 0x2d;
~     // 一元位补码(0s变为1s，1s变为0s)
<<    // 左移 a << 4 == 0x220
>>    // 右移 a >> a == 0x02
```

2. 条件表达式
```
condition ? expr1 : expr2;
expr1 ?? expr2;       // expr1为非空时返回其值，否则返回expr2值
```

3. 级联操作
用`..`表示，类似与lodash的_.chain(value).map().values();

#### 流程控制语句

1. if else
2. for
```
for (var i = 0; i < 5; i++){}
for (var i in array ) {}        // 遍历数组
```

3. while 和 do-while
```
var a = 0;
var str = '';
while(a < 5){
    str += a.toString();
    a++;
}
print(str);

do{
    str += a.toString();
    a++;
}
while (a < 5);
print(str);
两个例子结果都是01234
```

4. break 和 continue
`break`跳出循环
`continue`跳出本次循环
```
var arr = [0, 1, 2, 3, 4];
for (var v in arr){
    if(v == 2){
        // break;
        continue;
    }
    print(v);
}
当break时的结果是: 0, 1;
当continue时的结果是: 0, 1, 3, 4;
```
5. switch 和 case
6. assert (断言)
7. try-catch 和 throw

#### 异常处理

1. 抛出异常
稳定健壮的程序一定是做了大量异常处理的，所以建议你在编写程序时尽量考虑到可能发生的情况。
```
throw FormatException('抛出一个 FormatException 异常');
throw '数据非法!';
```

2. 捕获异常
第一个`catch`用来捕获异常详细信息，第二个`catch`是堆栈跟踪信息。
```
try {
    var a;
    throw '非法数据！';
} on Exception catch (e) {
    print('Exception detail: \n $e');
} catch (e, s) {
    print('Exception detail: \n $e');
    print('Stack trace: \n $s');
}
```

3. Finally
要确保某些代码能够运行，无论是否抛出异常，请使用finally子句。如果没有catch子句匹配异常，则异常在finally子句运行后传播。
```
 try {
    var a;
    throw '非法数据！';
} on Exception catch (e) {
    print('Exception detail: \n $e');
} catch (e, s) {
    print('Exception detail: \n $e');
    print('Stack trace: \n $s');
} finally {
    print('Do some thing');
}
```