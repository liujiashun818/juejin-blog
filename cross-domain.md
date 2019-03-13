### 一、jsonp ###
 * 缺点：只能 get，不支持post、put、delete
 * 不安全 xss
 ```
 // jsonp
  function jsonp({url,params,cb}) {
      return new Promise((resolve,reject)=>{
        let script = document.createElement('script');
        window[cb] = function (data) {
          resolve(data);
          document.body.removeChild(script);
        }
        params = {...params,cb} // wd=b&cb=show
        let arrs = [];
        for(let key in params){
          arrs.push(`${key}=${params[key]}`);
        }
        script.src = `${url}?${arrs.join('&')}`;
        document.body.appendChild(script);
      });
    }
    jsonp({
      url: 'http://localhost:3000/say',
      params:{wd:'我爱你'},
      cb:'show'
    }).then(data=>{
      console.log(data);
    });
 
 ```
### 二、cors ###
 * 后端设置
 ```
 <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <script>
    let xhr = new XMLHttpRequest;
    document.cookie = 'name=zfpx';
    xhr.withCredentials = true; //强制带上cookie
    xhr.open('PUT','http://localhost:4000/getData',true);
    xhr.setRequestHeader('name','zfpx');
    xhr.onreadystatechange = function () {
      if(xhr.readyState === 4){
        if(xhr.status>=200 && xhr.status < 300 || xhr.status ===304){
          console.log(xhr.response);
          console.log(xhr.getResponseHeader('name'));
        }
      }
    }
    xhr.send();
  </script>
</body>
</html>
 ```
 ```
 let express = require('express');
let app = express();
let whitList = ['http://localhost:3000']
app.use(function (req,res,next) {
  let origin = req.headers.origin;
  if(whitList.includes(origin)){
    // 设置哪个源可以访问我
    res.setHeader('Access-Control-Allow-Origin', origin);
    // 允许携带哪个头访问我
    res.setHeader('Access-Control-Allow-Headers','name');
    // 允许哪个方法访问我
    res.setHeader('Access-Control-Allow-Methods','PUT');
    // 允许携带cookie
    res.setHeader('Access-Control-Allow-Credentials', true);
    // 预检的存活时间 6秒内不接受请求
    res.setHeader('Access-Control-Max-Age',6);
    // 允许返回的头
    res.setHeader('Access-Control-Expose-Headers', 'name');
    if(req.method === 'OPTIONS'){
      res.end(); // OPTIONS请求不做任何处理
    }
  }
  next();
});
app.put('/getData', function (req, res) {
  console.log(req.headers);
  res.setHeader('name','jw');
  res.end("我不爱你")
})
app.get('/getData',function (req,res) {
  console.log(req.headers);
  res.end("我不爱你")
})
app.use(express.static(__dirname));
app.listen(4000);
 ```
### 三、iframe ###

* postMessage页面之间的互相通信
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <iframe src="http://localhost:4000/b.html" frameborder="0" id="frame" onload="load()"></iframe>
  <script>
    function load() {
      let frame = document.getElementById('frame');
      frame.contentWindow.postMessage('我爱你','http://localhost:4000');
      window.onmessage = function (e) {
        console.log(e.data);
      }
    }
  
  </script>
</body>
</html>
```
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <script>
    window.onmessage = function (e) {
      console.log(e.data);
      e.source.postMessage('我不爱你',e.origin)
    }
  </script>
</body>
</html>
```
* window.name 
```
//a.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  a和b是同域的 http://localhost:3000
  c是独立的  http://localhost:4000
  a获取c的数据
  a先引用c c把值放到window.name,把a引用的地址改到b
  <iframe src="http://localhost:4000/c.html" frameborder="0" onload="load()" id="iframe"></iframe>
  <script>
    let first = true
    function load() {
      if(first){
        let iframe = document.getElementById('iframe');
        iframe.src = 'http://localhost:3000/b.html';
        first = false;
      }else{
        console.log(iframe.contentWindow.name);
      }
    }
  </script>
</body>
</html>
//b.html 空
//c.html =
<body>
  <script>
    window.name = '我不爱你'  
  </script>
</body>
```
* location.hash
 ```
 // a.html
 <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <!-- 路径后面的hash值可以用来通信  -->
  <!-- 目的a想访问c -->
  <!-- a给c传一个hash值 c收到hash值后  c把hash值传递给b b将结果放到a的hash值中-->
  <iframe src="http://localhost:4000/c.html#iloveyou"></iframe>
  <script>
    window.onhashchange = function () {
      console.log(location.hash);
    }
  </script>
</body>
</html>

//b.html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <script>
    window.parent.parent.location.hash = location.hash  
  </script>
</body>

</html>
//c.html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body>
<script>
  console.log(location.hash);
  let iframe = document.createElement('iframe');
  iframe.src = 'http://localhost:3000/b.html#idontloveyou';
  document.body.appendChild(iframe);
</script>
</body>

</html>
 ```
 * domain
 ```
 a.html
 <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <!-- 域名 一级域名二级域名 -->
  <!-- www.baidu.com -->
  <!-- viode.baidu.com -->
  <!-- a是通过 http://a.zf1.cn:3000/a.html -->
  helloa
  <iframe src="http://b.zf1.cn:3000/b.html" frameborder="0" onload="load()" id="frame"></iframe>
  <script>
    document.domain = 'zf1.cn'
    function load() {
      console.log(frame.contentWindow.a);
    }
  </script>

</body>
</html>
//b.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
   hellob
   <script>
      document.domain = 'zf1.cn'
     var a = 100;
   </script>
</body>
</html>
 ```
### 四、websocket ### 
* 高级api ,不兼容，一般用（socket.io） 速度快，平等的双方的通信
* 没有跨域限制，需要服务端配合
* 发给服务器，服务器发送到另一个页面。服务端安装ws,
```
// 前端
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <script>
    // 高级api 不兼容 socket.io(一般使用它)
    let socket = new WebSocket('ws://localhost:3000'); //服务器
    socket.onopen = function () {
      socket.send('我爱你');
    }
    socket.onmessage = function (e) {
      console.log(e.data);// 服务器回复的信息
    }
  </script>
</body>
</html>
// 后端
let express = require('express');
let app = express();
let WebSocket = require('ws');
let wss = new WebSocket.Server({port:3000});
wss.on('connection',function(ws) {
  ws.on('message', function (data) {
    console.log(data);
    ws.send('我不爱你')
  });
})

```
### 五、nginx跨域 ###
 *新增加个json文件，写mock数据
 * 修改nginx 的配置文件conf, 新增内容为： loactin ~.*\.json{
     root json;
    add_header 'Access-Control-Allow-origin' '*'; // 解决跨域
 }
 
* 重启下nginx就可以
* 在默认文件目录下添加json文件的路由，就可以获取json文件的内容(此为不跨域)。
* 如果其它地址请求会出现跨域问题。解决办法为：添加 add_header 'Access-Control-Allow-origin' '*';
