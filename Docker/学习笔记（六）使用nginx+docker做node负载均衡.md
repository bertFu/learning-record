# 使用nginx+docker做node负载均衡

## 配置Nginx

修改nginx.conf：

```
http{
  ....
    upstream sample {
     server 127.0.0.1:3000;
     server 127.0.0.1:3001;
     keepalive 64;
    } 
     server {
      listen 80;
      ....
      server_name 127.0.0.1;
      ....
      location / {
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header Connection "";
        proxy_http_version 1.1;
        proxy_pass http://sample;
      }
    }
}
```

这里在3000端口和3001端口各有一个Node.js服务器，这两个服务器在做同样的工作。在upstream节，配置了两个Node.js服务器。此外，我们还设置了proxy_pass <http://sample做HTTP请求代理。>

启动 `nginx`:

```
sudo nginx
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
```

## 构建NodeJS服务器

创建 `test.js` 文件

```
var http = require('http');
var morgan    = require('morgan');

var server1 = http.createServer(function (req, res) {
 console.log("Request for: " + req.url + "-- port 3000 ");
 res.writeHead(200, {'Content-Type': 'text/plain'});
 res.end('Hello Node.js\n');
}).listen(3000, "127.0.0.1");

var server2 = http.createServer(function (req, res) {
 console.log("Request for: " + req.url + "-- port 3001 ");
 res.writeHead(200, {'Content-Type': 'text/plain'});
 res.end('Hello Node.js\n');
}).listen(3001, "127.0.0.1");

server1.once('listening', function() {
 console.log('Server running at http://127.0.0.1:3000/');
});

server2.once('listening', function() {
 console.log('Server running at http://127.0.0.1:3001/');
});
```

初始化 `package.json`

```
npm init
```

安装依赖模块

```
npm install http
npm install morgan
```

启动 `node`:

```
node test.js
```

## 访问Nginx服务器

现在我们可以访问<http://127.0.0.1> 可以看到如下的输出：

```
Server running at http://127.0.0.1:3000/
Server running at http://127.0.0.1:3001/
Request for: /-- port 3001  
Request for: /favicon.ico-- port 3000  
Request for: /favicon.ico-- port 3001  
Request for: /-- port 3000  
Request for: /favicon.ico-- port 3001  
Request for: /favicon.ico-- port 3000  
Request for: /-- port 3001  
Request for: /favicon.ico-- port 3000  
Request for: /favicon.ico-- port 3001  
Request for: /-- port 3000  
Request for: /favicon.ico-- port 3001  
Request for: /favicon.ico-- port 3000
```

可以看到连续两次请求在轮流访问两个服务

这个时候在看看直接访问node服务 <http://127.0.0.1:3000/>

```
Request for: /-- port 3000
Request for: /favicon.ico-- port 3000
```

两次访问同一个服务，说明成功使用 `nginx` 做负载均衡
