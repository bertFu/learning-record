# 创建一个Web应用的发布Image

这里以Node.js为案例，适合其他web.这里以Ubuntu 14.04 amd64.为基础，获得ubuntu的最新images:

```
$ docker pull ubuntu:14.04
```

启动一个新容器：

```
$ docker run -ti ubuntu:14.04 bash
```

在这个新容器中，安装node.js:

```
$ apt-get update && apt-get install -y nodejs
```

创建应用内容在/server.js文件中：

```
var http = require('http');
var os = require('os');

var server = http.createServer(function (request, response) {
      response.writeHead(200, {"Content-Type": "text/plain"});
      response.end("Hello from " + os.hostname() + "\n");
});

server.listen(8000);

console.log("Server running at http://127.0.0.1:8000/");
```

在容器中启动脚本/run.sh：

```
#! /bin/sh
/usr/bin/nodejs /server.js

$ chmod a+x /run.sh
```

停止这个容器，为其创建一个新的部署包image:

```
$ exit

# Get the ID of the container
$ sudo -E docker ps -a

# Change 3796ab3f5b76 in the following command with the ID listed above
$ sudo -E docker commit 3796ab3f5b76 local/nodeapp

# Remove the old container
$ sudo -E docker rm 3796ab3f5b76
```

安装synapse

```
$ sudo apt-get install build-essential ruby ruby-dev haproxy
$ sudo gem install synapse
```

编辑/etc/default/haproxy 设置 ENABLED 为 1

# 启动后端实例

启动一个web应用容器实例：

```
$ sudo -E docker run -d -p 8000 local/nodeapp /run.sh
```

测试是否启动成功，首先我们要获得Docker对外暴露的端口，一般是8000，这里是49153:

```
$ sudo docker ps
$ curl http://127.0.0.1:49153
```

应该得到响应：`Responds with "Hello from {container_id}"`

# 使用Synapse自动配置HAproxy

下面到了最关键的服务自动发现环节。使用下面内容创建一个文件/etc/synapse.json.conf ：

```
{
  "services": {
    "nodesrv": {
      "discovery": {
        "method": "docker",
        "servers": [
          {
            "name": "localhost",
            "host": "localhost"
          }
        ],
        "container_port": 8000,
        "image_name": "local/nodeapp"
      },
      "haproxy": {
        "port": 8080,
        "listen": [
          "mode http",
          "option httpchk /",
          "http-check expect string Hello"
        ]
      }
    }
  },
  "haproxy": {
    "reload_command": "service haproxy reload",
    "config_file_path": "/etc/haproxy/haproxy.cfg",
    "do_writes": true,
    "do_reloads": true,
    "global": [
      "chroot /var/lib/haproxy",
      "user haproxy",
      "group haproxy",
      "daemon"
    ],
    "defaults": [
      "contimeout 5000",
      "clitimeout 50000",
      "srvtimeout 50000"
    ]
  }
}
```

解释配置如下：

1. services.nodesrv.discovery: 这是watcher的配置.这里我们使用 Docker API来发现一个名为local/nodeapp的应用容器，端口在8000.
2. services.nodesrv.haproxy: 与 HAproxy相关配置，配置其和nodesrv服务相关的端口.
3. haproxy: 由Synapse管理的全局 HAproxy 实例配置。

启动HAproxy 和 Synapse

```
$ sudo service haproxy start
$ sudo synapse -c /etc/synapse.json.conf
```

通过HAProxy访问测试，这时它的端口是8080

```
$ curl http://localhost:8080
```

得到的响应是：`Responds Hello from {container_id}` 下面再次启动nodeapp的第二个Docker：

```
$ sudo -E docker run -d -p 8000 local/nodeapp /run.sh
```

然后在测试HAproxy，过了几秒会发现两个节点服务器都有了响应。

下面我们测试如果一个节点关闭，是否会影响正常访问，这实际就是失败容错failover。

下面命令是每过2秒循环调用一个HAproxy:

```
while :
do
    curl http://localhost:8080
    sleep 2
done
```

然后停止其中一个容器：

```
$ sudo -E docker stop {container_id}
```

过了几秒以后，只有一个服务器在响应。

# 相关参考：

[Docker微容器+微服务将颠覆传统的软件架构](http://www.jdon.com/46474)
