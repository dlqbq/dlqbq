---
title: JavaScript去中心化p2p实现之bugout
date: 2024-06-04 00:00:00
---
### 安装
``` shell
npm i bugout
```

或者使用script

``` html
<script src="https://chr15m.github.io/bugout/bugout.min.js"></script>
```

### 使用
导入Bugout

``` javascript
var Bugout = require('bugout')
```

创建Bugout服务器：

``` javascript
// 实例化Bugout
var  b  = new Bugout（)
//  获取要与客户端共享的服务器地址（公钥哈希）
//  这是客户端将用于连接回该服务器的内容

//  注册远程用户可以进行的API调用
b.register('ping', function(address, args, callback) {
  //  修改传递的参数并回复
  args.hello = 'Hello from ' + b.address();
  callback(args)
})

//  保存此服务器的会话密钥种子以重新使用
localStorage['bugout-server-seed'] = b.seed
```

客户端与服务器端链接

``` javascript
var b = new Bugout('服务器的公钥')
 
//  等待，直到我们看到服务器
//  （可能要花一分钟时间才能穿过防火墙等）
b.on('server', function(address) {
  //  一旦我们可以看到服务器
  // 对其  进行API调用
b.rpc('ping', {'hello': 'world'}, function(result) {
    console.log(result);
    //  {“ hello”：“ world”，“ pong”：true}
    //  同时检查result.error
  });
});
 
//  保存此客户端实例的会话密钥种子以重新使用
localStorage['bugout-seed'] = JSON.stringify(b.seed);
```

客户端和服务器与其他连接的客户端进行交互

``` javascript
//  从服务器接收所有带外消息
//  或其他任何连接的客户端
b.on('message', function(address, message) {
  console.log('message from', address, 'is', message);
});
 
//  只要我们在这个群组中看到新客户
b.on('seen', function(address) {
  // e.g. send a message to the client we've seen with this address
});
 
//  您也可以关闭Bugout频道以停止接收消息等。
b.close();
```

所有人连接共同的Bugout服务器

``` javascript
var b = new Bugout('特定的共享标识符')
```
