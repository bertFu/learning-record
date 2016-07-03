### Docker的基本操作

创建docker
```
docker create imageName
```
使用 `create` 创建的 `Docker` 容器不会启动， `imageName` 是镜像名称，需要通过镜像来启动容器，`Docker` 会先在本地搜索该镜像，本地没有时会到公有仓库下载，并将其添加到本地。

如果要启动该容器使用:  
```
docker start id/name
```
用过 `docker start` 来启动容器，通过 `容器的ID` 或者 `容器的名字` 来启动  

```
docker ps     // 查看正在运行的容器
docker ps -a  // 查看所有容器，包括正在运行中的，和关闭状态下的容器 
```
  
关闭正在运行的容器
```
docker stop
docker kill
```
容器创建后就与终端无关，即使关闭了，该容器依然存在  

```
sudo docker run -i -t --name=dockerName imageName /bin/bash
```
`-i`：打开容器的标准输入  
`-t`：告诉 `Docker` 为容器创建一个命令行终端  
这两个标志为我们和容器提供了交互 `shell` 是创建容器的基本配置
  
`--name`：为容器命名，当没有使用这个标记时，`Docker` 会随机为容器命名  
`/bin/bash`：`Docker` 创建成功后会进入容器内的终端，出现：
```
root@7238r37453443:/#
```
`@` 前面是我们登入的 `root`，后面是容器的主机名  
需要退出容器可以使用 `ctrl + D` 或者 `exit` 命令

```
docker ps -l // 查看最后创建的容器
docker ps -n=x // 查看 `x` 个数的容器
```

```
docker run --restart=always --name=dockerName -d imageName /bin/bash
```
`--restart` 标志被设计成 `always` 表示：不管容器返回码是什么，`Docker` 都会尝试重启容器  
`on-failure` 表示：当容器的返回值是非0时， `Docker` 才会重启容器   
`on-failure` 标志接受一个可选重启次数  
```
--restart=on-failure:5
```
表示最多尝试重启容器5次  

删除容器：
```
docker rm dockerId/dockerName // 删除指定容器
docker rm `docker ps -a -q` // docker没有提供一次性删除所有容器的命令，可以通过此命令来实现删除所有容器
```
`-q` 表示列出容器的Id， `docker ps -a -q` 表示查询出所有容器，列出其ID  

终端依附到交互型容器中
```
docker arrach dockerId/dockerName
```
使用 `attach` 依附到容器后要多按一次回车才会出现容器的 `shell` 交互页面    

查看 `Docker` 日志输出
```
docker run -d --name deamon_logs ubuntu:14.04 /bin/bash -c 'for((i=0;1;i++));do echo $i; sleep 1;done'
docker logs -f dockerId/dockerName
```
`logs` 输出是从容器启动到调用执行logs命令时的所有输出，之后的日志不在输出，并立即返回主机的控制台  
使用 `-f` 可以看到日志，并在不断更新。  

```
docker logs -f --tail=5 deamon_logs
docker logs -f --tail=5 -t deamon_logs 
```
`--tail`可以限定日志输出的长度  
`-t` 可以打印出日志的产生时间  

```
docker top deamon_logs
```
可以看到容器的进程

```
docker inspect deamon_logs
```
查看容器信息



