# 神奇的 for 循环

## 小二先上点茶水

猜猜会发生什么：

```javascript
var funcs = [];

for ( var i = 0 ; i < 10 ; i ++ ) {
    funcs.push( function() {
        console.log(i);
    })
}

funcs.forEach( function ( func ) {
    func();
});

```

*例子来自《深入理解ES6》第一章 第8页*

这是道 JS 的入（song）门（ming）题。

打印出来的答案是：

![](http://image.yidaqiang.cn/blog/js/for-1.png)

没想到吧！

## 等等，小二我要的不是啤酒

为什么会这样呢？

这里先解释一下，这段函数干了啥。这段代码主要就是构建了一个函数数组，存放了十个函数。再`for-each`遍历执行。

![](http://image.yidaqiang.cn/blog/js/for-2.png)

然而在数组里面的函数体却是`console.log(i)`。这里的`i`是`for ( var i ; i < 10 ; i ++)`处的引用，可以理解为“这里的`i`”和`for`申明的的`i`是同一个变量。`for`循环结束`i`变为`10`也导致，数组里的每个函数的函数体`console.log(i)`里的`i`也都变为了`10`。

断点查看一下：

![](http://image.yidaqiang.cn/blog/js/for-3.png)

此时，`funcs`数组里有的一个函数。控制台执行这个函数。

![](http://image.yidaqiang.cn/blog/js/for-4.png)

这时打印的值不是一开始的 0 ，而是 1 了。

同理第一个for循环结束以后就变为了 i 变为了 10 ，所有叫 i 的变量都是 10 了。

代码改成这样大概更好理解：

```javascript
var funcs = [];

var i;

for ( i = 0 ; i < 10 ; i ++ ) {
    funcs.push( function() {
        console.log(i);
    });
}

funcs.forEach( function ( func ) {
    func();
});

```

`i`的声明放到前面，实际上后`for-each`执行`func`访问的就是这个变量`i`。显然，结果的打印 10 次 10。一开始的代码等价于上面的代码。

究其原因是因为 JS 的**变量提升机制**。这个机制有时很恶心，因为它可以把作用域搞的很乱，你不得不小心翼翼的处理这个问题。上面的例子就是一个典型。

## 来点饮料吧

解决方案1：
```javascript

var funcs = [];

var i;

for ( i = 0 ; i < 10 ; i ++ ) {
    funcs.push( ( function(value) {
        return function() {
            console.log(value);
        }
    } ( i ) ) );
}

funcs.forEach( function ( func ) {
    func();
});

```

这里同时使用立即执行函数和闭包。

在 ES6 有更简洁实现方式。

方案2：

```javascript
var funcs = [];

for ( let i = 0 ; i < 10 ; i ++ ) {
    funcs.push( function() {
        console.log(i);
    });
}

funcs.forEach( function ( func ) {
    func();
});

```

这里每次使用`let`声明，每次循环都会重新创建一个新变量。就不存在 ES5 之前的问题。

## 最后来杯清水

ES6 引入了很多新的特性，来解决之前 JS 存在的问题。积极拥抱 ES6 时代的JS。
