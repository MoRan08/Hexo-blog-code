### **JS系列之**apply与call，bind **详解**

------

​		在JavaScript 中，call、apply 和 bind 是 Function 对象自带的三个方法，这三个方法的主要作用是改变函数中的 this 指向，从而可以达到`接花移木`的效果 

## 一、this

this 指的是某一对象。但是在不同的场景，this表示的是不同的对象。

下面大概分为5种情况：

### 1、window

```
// 第一种：this 指的就是 window
function fn() {
    console.log(this);
};
console.log(this);
```

### 2、事件中的this

```
// 第二种：所有事件函数中的this，都是这个监听事件监听对象
//在事件函数中 this===e.currentTarget 也就是事件侦听的对象。在事件中使用this，其实事件函数内部实现极力就是使用call等将事件中的this改为 e.currentTarget。
// currentTarget始终是监听事件者，而target是事件的真正发出者。
document.querySelector(".div0").onclick=function (e) {
    console.log(this);
    console.log(e.target);
    console.log(e.currentTarget);
};
```

### 3、对象中的this

```
// 第三种：对象中的this，this就是当前这个对象。
// 在对象的函数中，如果需要调用当前对象的其他函数或者属性时，必须加 this.属性名 或者 this.方法名.
var obj={
    a:1,
    c:function () {
        console.log(this.a);
    }
};
```

### 4、面向对象中的this

```
// 第四种：面向对象中的this
// ES5 面向对象写法
function Box(d) {
    this.d=d;
}
Box.prototype={
    a:1,
    c:function () {
	    //  这里的this就是box1
        console.log(this.a+this.d);
    }
};
var box1=new Box(10);
box1.c();

// ES6 的面向对象写法
class Ball{
    constructor(d){
        this.d=d;
        this.a=1;
    }
    c(){
        console.log(this.d+this.a)
    }
}

var ball1=new Ball(10);
ball1.c();
```

### 5、call，apply和bind中的this

```
//第五种：call，apply和bind中的this
// 这里的this就是被替换的对象
function fn2(a,b) {
    this.a=a;
    this.b=b;
    return this;
}

var obj1=fn2.call({},3,5);
var obj2=fn2.call({c:6},10,12);
console.log(obj1,obj2);// {a:3,b:5}, {a:3,b:5,c:6}
```

## 二、apply 和 call 方法

**语法**：

```
//1、apply的使用语法：
函数名.apply(对象，[参数1， 参数2，... ])；
方法名.apply(对象，[参数1， 参数2，... ])；

//2、call的使用语法：
函数名.call(对象，参数1， 参数2，... )；
方法名.call(对象，参数1， 参数2，... )；
```

不同的是传入参数时，apple有两个参数，第二个参数是数组；call 从第二个参数开始是调用其的函数的所有参数。

> apple 和 call 都可以改变调用其的函数或方法中的 this 指向。this的指向为传入的对象。
>
> 当传入对象的位置为 null 时，不改变this的执行，此时相当于函数的一般执行。

### 1、函数调用apply和call

```
function f1(x, y) {
  console.log(x+y +this); // 这里面的this是window
  return x+y;
}

var r1 = f1.apply(null, [10,20]); // 打印30 window，传入的是null，所以this指向还是window
console.log(r1); // 30
var r2 = f1.call(null, 10,20);// 打印30 window
console.log(r2); // 30
```

```
//函数改变 this 的指向
var obj = {};

var r1 = f1.apply(obj, [10,20]); // 打印30 window，传入的是Obj，所以this指向是Obj
console.log(r1); // 30
var r2 = f1.call(obj, 10,20);// 打印30 Obj
console.log(r2); // 30
```

示例：

```
// 改变函数fn的this为array
function fn(a, b) {
    this.push(a, b);
    console.log(this);
};

fn.call(new Array, 1, 2);//[1, 2]
```

### 2、方法调用apply和call

```
// 方法改变 this 的指向
    function Person(age) {
        this.age = age;
    }
    Person.prototype.eat = function () {
        console.log(this.age); // this 指向实例对象
    };

    function Student(age) {
        this.age = age;
    }

    var per = new Person(18);
    var stu = new Student(20);

    per.eat.apply(stu); // 打印 20
    per.eat.call(stu); // 打印 20
```

> 由于 eat 方法已经指向了 Student 了，所以打印 20，而不是 18.

问题：我们知道函数也是对象，函数可以调用 apple 和 call 方法，但是这两个方法并不在这个函数这个对象的实例函数中，那么在哪里呢？

> **解答：**所有的函数都是 Function 的实例对象，而 apply 和 call 就在 Function 构造函数的原型对象中。

## 三、bind方法

bind 是复制的意思，也可以改变调用其的函数或方法的 this 指向，参数可以在复制的时候传进去，也可以在复制之后调用的时候传进去。

如果一个函数fn中的this是window，那么fn.bind(obj)就把fn中的this改为obj。但是fn还是一样的执行。

fn.bind(obj)();

所以bind就是改this，没有其他。

> 使用语法：
>
> 1、函数名.bind(对象, 参数1, 参数2, ...); // 返回值是复制的这个函数
>
> 2、方法名.bind(对象, 参数1, 参数2, ...); // 返回值是复制的这个方法

### 1、函数调用 bind

```
function f1(x, y) {
    console.log(x + y + this);
}
// 1.参数在复制的时候传入
var ff = f1.bind(null,10,20); // 这只是复制的一份函数，不是调用，返回值才是
ff(); 
或者 f1.bind(null)(10,20);

// 2.参数在调用的时候传入
var ff = f1.bind(null); // 这只是复制的一份函数，不是调用，返回值才是
ff(10,20);
```

### 2、方法调用 bind

```
function Person(age) {
    this.age = age;
}

Person.prototype.eat = function () {
    console.log(this.age); // this 指向实例对象
};

function Student(age) {
    this.age = age;
}

var per = new Person(18);
var stu = new Student(20);

var ff = per.eat.bind(stu);
ff(); // 20
```

### 3、call，apply和bind的区别：

> call,apply 一调用函数就执行了，而bind调用了函数并没有执行。

## 四、闭包

### 1、闭包的概念

闭包是指有权访问另一个函数作用域中的变量的函数。因为js的ES5自身没有面向对象，因此变量和函数通常都是写在同一个空间中，这样就会造成变量，函数名称污染（重名），为了减少这种重名，我们使用创建一种封闭空间完成局部私有变量的创建，并且这个局部私有变量不会再函数运行完成后被清理，能够长期使用。这就是闭包。

形成一个闭包很简单：创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量。比如：有一个函数 A 中有一个函数或者对象 B，那么函数或者对象 B 可以访问函数 A 中的数据，那么函数 A 的作用域就形成了闭包。

也就是要求：

- 必须有两个函数，并且是嵌套关系，外面的函数必须返回里面的函数。
- 在全局中必须接收返回函数作为变量存储.

### 2、闭包的模式

函数模式的闭包：函数中包含函数。

```
function fn1() {
    var i=0;
    return function () {
        i++;
        console.log(i);
    }
}
// fn1的返回值是一个函数，函数也是对象，存在堆中。那么与此函数相关的内容，也会被存放在堆中。就不容易被清除。
var fn2=fn1(); 
fn2(); // 1
fn2(); // 2
fn2(); // 3
```

对象模式的闭包：函数中包含对象。

```
var method=(function () {
    var i=0;
    return {
        a:1,
        b:2,
        c:function () {
            i++;
            console.log(i);
        }
    }
})();
method.c(); // 1
method.c(); // 2
method.c(); // 3
```

面向对象方式的闭包：

```
var Box=(function () {
    function Box() {

    }
    Box.prototype={

    };
    return Box;
})()
```

### 3、闭包的特点

- 函数嵌套函数
- 函数内部可以引用外部的参数和变量
- 参数和变量不会被垃圾回收机制回收

### 4、闭包的优缺点

也是缓存的数据，导致在闭包的范围内一直起作用，造成内存泄漏，不会被垃圾回收。

### 5、闭包的应用

缓存数据，函数中的数据，外面可以使用。

如果想要缓存数据，就把这个数据放在外层的函数和里层的函数之间。这样不停的调用里层函数，相当于外层函数里的数据没有得到及时释放，就相当于缓存了数据。

```
// 函数闭包
function A() {
    var num = 10;
    return function () {
        return num++;
    }
}

var func = A();
console.log(func());
console.log(func());
console.log(func());
```

```
// 对象闭包
function A() {
    var num = 10;
    return {
        age: num++
    };
}
var func = A();
console.log(func.age);
```

## 五、沙箱

沙箱：一小块的真实环境，里面发生的事情不会影响到外面。相同的操作，相同的数据都不会和外面发生冲突。

作用：避免命名冲突。

比如：**自调用函数**里面就相当于一个沙箱环境。

```
(function (){
        
}());
```