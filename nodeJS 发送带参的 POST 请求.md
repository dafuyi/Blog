# nodeJS 发送带参的 POST 请求

## 开头总要有几句话

nodeJs 非常容易的可以进行路由分配，发送相应的 HTML 代码。但，实际运用中涉及到了 nodeJS 的另一个主要用途就是做一个中间层。这就要求到了用 nodeJS 做 http 请求。也就是说：前端发送请求到 nodeJS 指定的 URL 地址，nodeJS 在处理这个 URL 时向后端发送一个数据请求，最后把从后端得到的数据做一层渲染或处理。所以，如何在 nodeJS 里发起一个 http 请求呢？

<!--more-->

## 使用到的方法

`http.request()`在 nodeJS 里主要用于发起 http 请求的方法。

使用时需要传入一个`options`对象和一个回调函数，返回一个对象 `http.clienRequest`对象。

这个返回的对象可以通过进一步事件监听来处理事务逻辑。

### 被坑的地方

在网上找了很多的代码，都是在传入的`http.request()`的回调函数的里监听`res`对象。我遇到过一个迷之 BUG。昨天，我在这个回调函数用外层的`res.send()`方法来发送请求，它不行！刚刚试了一下好像又可以了！！它可以了！！！由于年代久远，没有前面的代码对比，这当一个未解之谜吧。

我在 API 文档里看了半天找到另一种解决方式。把`http.request()`方茴的对象定义为`send`(避免变量名重名)。监听`send`对象的`reponse`和`end`事件。

## 小二！上代码

**外层函数，下面的都是放在这个函数里的**

```javascript
var express = require('express');
var router = express.Router();
var http = require('http');
var querystring = require('querystring');

router.get('/', function(req, res, next) {
    // 下面代码放这里
}

module.exports = router;
```
**方案一：**

```javascript
var postData = querystring.stringify({
        jsonList: '这是测试数据6666'
});


var  options = {
    hostname: '127.0.0.1',
    port: 3000,
    path: '/formSql/insertData',
    method: 'POST',
    headers: {
        'Content-Type':'application/x-www-form-urlencoded',
        'Content-Length': Buffer.byteLength(postData)
    }
};
var getData = '';
var send = http.request(options, function (innerres){
    innerres.setEncoding('utf8');
    var data = '';
    innerres.on('data', function(chunk){
        data += chunk;
    });
    innerres.on('end', function (){
        res.send(chunk);
        console.log('响应结束！');
    });
});

send.write(postData + '\n');
send.end();

```

**方案二：**
```javascript

var postData = querystring.stringify({
        jsonList: '这是测试数据8888'
});

var  options = {
    hostname: '127.0.0.1',
    port: 3000,
    path: '/formSql/insertData',
    method: 'POST',
    headers: {
        'Content-Type':'application/x-www-form-urlencoded',
        'Content-Length': Buffer.byteLength(postData)
    }
};
var getData = '';
var send = http.request(options);

send.write(postData + '\n');
send.on('response',function(onres) {
    console.log('on response');
    var data = '';
    onres.on('data',function(chunk) {
        data  += chunk;
    });
    onres.on('end',function() {
        res.send(data);
    })
});
send.on('error', function (e) {
    console.error('请求遇到问题: ' + e.message);
});
send.end();
```
