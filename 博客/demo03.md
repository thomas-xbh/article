### 前言
socket.io是一个基于websocket的javascript库,在浏览器中运行的客户端库，和一个面向Node.js的服务端库。两者有着几乎一样的API。基于websocket封装了很多请求,让实时通信更加简单
### 环境
express结合socket.io,前端原生
### 开始
1. 安装express,socket.io
> npm install express@4
> npm install socket.io
### 客户端
1. 新建一个index.html,这里采用的是socket.io官网中的例子
```
<ul id="messages"></ul>
    <form id="form" action="">
      <input id="input" autocomplete="off" /><button id="btn">Send</button>
    </form>
    <script src="https://cdn.socket.io/4.5.4/socket.io.min.js"></script>
    <script>
      var socket = io();
      var form =document.getElementById("form");
      var input = document.getElementById("input");
      var btn = document.getElementById("btn");
      var messages = document.getElementById("messages");

      btn.addEventListener("click", function(e){
        e.preventDefault();
        if(input.value){
            //注册消息/发送消息
            socket.emit("chat message", input.value);
            input.value=''
        }
      })
        /* 接收/监听消息 */
      socket.on('chat message',function(msg){
        var item=document.createElement('li')
        item.textContent=msg
        messages.appendChild(item)
        window.scrollTo(0,document.body.scrollHeight)
      })
    </script>
```
### 服务端
1. 创建index.js文件
```
const express = require("express");
const path = require("path");

const app = express();
const server = require("http").createServer(app);
const { Server } = require("socket.io");
const io = new Server(server);

app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "index.html"));
});
// 连接
io.on("connection", (socket) => {
  console.log("a user connected");

//   广播消息
  socket.broadcast.emit('chat message','hello welcome');

//   监听消息/接收消息
  socket.on("chat message", (data) => {
    // console.log('chat message',data);
    io.emit('chat message',data);
    
  });
//   断开连接
  socket.on('disconnect', () => {
    console.log('user disconnected');
  });
});
server.listen(3000, function () {
  console.log("listening on port 3000");
});

```
可以通过node运行index.js,访问本地3000端口
![image.png](https://s2.loli.net/2023/08/18/6qJ3Ld8tUSAasvR.png)
可以多开几个窗口,当另一个用户进入会发送hello welcome,每个用户都可以发消息
![image.png](https://s2.loli.net/2023/08/18/oEZ9ieHQr6BpYfh.png)

