---
title: JavaScript Async （异步）
permalink: untitled-9
id: 16
updated: '2015-01-27 22:08:13'
date: 2015-01-27 22:07:39
tags:
---

今天无意间关注了一些 [JavaScript 的知识](http://net.tutsplus.com/tutorials/javascript-ajax/event-based-programming-what-async-has-over-sync/)。由于我本身并不太了解 JavaScript ，因此以下内容也仅仅作为学习探讨之用。其中，我主要对 JS 的异步编程思想比较感兴趣，毕竟 *能写出漂亮的异步代码* 也算得上是一个难点。况且，仅仅知道如何使用而不了解其实现原理，有时候总觉得心里堵得慌。

---

## [setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/window.setTimeout?redirectlocale=en-US&redirectslug=DOM%2Fwindow.setTimeout) & [setInterval()](https://developer.mozilla.org/en-US/docs/Web/API/window.setInterval?redirectlocale=en-US&redirectslug=DOM%2Fwindow.setInterval)
这两个函数是 JS 里面异步思想的典型代表。setTimeout() 可以设置在一定时间之后执行某个函数，用法如下：
```javascript
console.log( "a" );
setTimeout(function() {
    setTimeout(function() {
        console.log( "f" )
    }, 500 );
    console.log( "c" )
}, 500 );
setTimeout(function() {
    console.log( "d" )
}, 500 );
setTimeout(function() {
    console.log( "e" )
}, 500 );
console.log( "b" );
```

<!--more-->

### 背后机制
比较有意思的是，setTimeout 这类有 *回调函数* 的函数，实际上是把 *回调函数* 放入了一个 **Event Loop** 队列中，这个队列遵循 FIFO ，即“先进先出” 策略。同时，只有当 setTimeout 后面的代码都执行完毕后， **Event Loop** 中的函数才会被处理。因此实际上上面这个例子的输出结果应为：
```shell
    >>>
a
b
c
d
e
f
```
也就是说f会在最后输出。这也验证了之前的说法。

---

## Ajax
Ajax 可以算是 JavaScript 里面用得比较多的，其全称是 *Asynchronous JavaScript and XML*。从名字来看就知道它实现了异步处理的机制。Ajax 使得浏览器不需要重新加载页面就能够从服务器获取新的内容并添加到当前页面上，这一招对于用户来说可谓是极好的，你总不希望填表格填到一半，突然页面刷新加载了一堆新的内容吧。利用 jQuery ，我们能够很方便的使用 Ajax，像下面这样：
```javascript
var data;       
$.ajax({
    url: "some/url/1",
    success: function( data ) {
        // But, this will!
        console.log( data );
    }
})
// Oops, this won't work...
console.log( data );
```
注意，这里也看得出 ajax 是异步的。返回的数据并不是马上可用，必须采用 success 这样的回调方法才可以。

### 背后机制
这里先记录一下听得比较多但是不知道是什么的 XHR，全称是 XMLHttpRequest，也就是用来向服务器发送请求获取数据的。尽管名字是 XML 开头，但是它支持 HTML 甚至二进制文件格式，同时除了 HTTP 外也支持 FTP 甚至 File 等协议。用法很简单：创建一个 XHR 实例，打开一个URL，然后发送请求。返回的 HTTP Status（也就是常见的200， 404， 502等状态）以及数据内容都可以在 responseXML/responseText 里面找到。

而 $.ajax 也正是利用了 XHR 来实现其功能。以下是实际上发生的。
```javascript
xmlhttp.open( "GET", "some/ur/1", true );
xmlhttp.onreadystatechange = function( data ) {
    if ( xmlhttp.readyState === 4 ) {
        console.log( data );
    }
};
xmlhttp.send( null );
```

---

## 暴露的问题
回调函数是很多异步实现的基础，但是如果迭代次数过多就会让代码变得很难看：
```javascript
GMaps.geocode({
    address: fromAddress,
    callback: function( results, status ) {
        if ( status == "OK" ) {
            fromLatLng = results[0].geometry.location;
            GMaps.geocode({
                address: toAddress,
                callback: function( results, status ) {
                    if ( status == "OK" ) {
                        toLatLng = results[0].geometry.location;
                        map.getRoutes({
                            origin: [ fromLatLng.lat(), fromLatLng.lng() ],
                            destination: [ toLatLng.lat(), toLatLng.lng() ],
                            travelMode: "driving",
                            unitSystem: "imperial",
                            callback: function( e ){
                                console.log( "ANNNND FINALLY here's the directions..." );
                                // do something with e
                            }
                        });
                    }
                }
            });
        }
    }
});
```
{% blockquote  Async Javascript http://pragprog.com/book/tbajs/async-javascript %}
The problem isn’t with the language itself; it’s with the way programmers use the language
{% endblockquote %}
解决方法就是采用有名函数而非匿名函数，从而尽量避免超过两重的回调函数嵌套。

---

## 其他相关的 JavaScript 异步资源
+ [async.js](https://github.com/caolan/async) 增强 JavaScript 的异步功能，例如异步平行、异步序列等功能。
+ [Promises](http://www.html5rocks.com/en/tutorials/es6/promises/#disqus_thread)
> A promise represents the eventual value returned from the single completion of an operation.

+ **Events** 事件是非常好的处理回调函数的方法。之前我在一篇文章中也看到， *事件能够有效的将两个对象分离* 。也就是比起直接的回调函数，事件监听这种方法（使用第三方来充当中间人进行连接）能够使得原本耦合的双方有了更多的自由。