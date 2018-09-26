---
title: 浅谈 null & undefined
date: 2018-09-26
categories:  
- JS
tags:
---

#### 1.初识 null & undefined

> 在javascript 中有两种特别的基本数据类型 null undefined 初学者 对其也很模糊或者直接认为它俩相等。

确实在判断 是否为真值时null 和undefined 也就是if语句中 它俩都是为 false, 甚至有

```
console.log( null == undefined ) // true
```

#### 2. 深入理解 undefined & null

在js中我们经常用一个 typeof来检测一个变量的类型， 而且返回的是一个字符串类型。看下面的例子

```
console.log( null === undefined ) // true? X
```

答案是否定的. 我们试着用 typeof 打印一下 null 和undefined

```
console.log( typeof null)                  // object
console.log( typeof null === "object")      // true
console.log( typeof undefined )              // undefined
console.log( typeof undefined === "undefined" )   // true undefined
```

我们发现 null 打印的是 object对象 而 undefined 打印的是undefined. (对于null 打印出object 有兴趣的可以去看看《你不知道的javaScript》中卷 第一章)

#### 3. 用法

> null: 表示 "没有对象", 也就是不应该有值。

1. 作为函数参数， 表示该函数的参数不是对象
2. 对象原型链的终点。 也是null .对[原型链](https://github.com/mqyqingfeng/Blog/issues/2)不熟悉的同学可以看看.

```
console.log(Object.prototype.__proto__ === null) // true
```

> undefined: 表示 没有值 缺少值 就是此处应该有个值但是没有定义

1. 变量被申明了但是没有被赋值

   ```
       var a ;
       console.log( a )   // undefined
       a = 2;
       console.log( a )   // 2
   ```

2. 函数调用时，该提供的参数没有提供。

   ```
     function f(a) {
   console.log( a );           // undefined
   }
   f();
   ```

3. 对象属性没有赋值， 该属性为undefined

   ```
   var obj = new Person();
   console.log(obj.age);      // undefined 
   ```

4. 当函数没有返回值时，默认返回undefined

   

   ```
   var f = fn();
   console.log( f );   // undefined
   ```