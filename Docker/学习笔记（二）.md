### 容器的导入和导出

先创建一个容器
```
& sudo docker run -it --name=inspect_import ubuntu:14.04 /bin/bash

root@1cbdcc18e616:/# 
```
然后按需要修改容器，安装需要的软件，配置系统环境。当我们完成这一切后，就可以吧容器保存到本地，使用 `docker export` 命令导出容器：
```
& docker export inspect_import > my_container.tar 

my_container.tar
``` 
`docker export` 命令会吧容器的文件系统以 `tar` 的格式导出到标准输出，使用 `>` 命令将其定位到目标文件 `name.tar`。将容器保存到本地之后，我们就可以通过网络等方法将 `tar` 包分享给他人。  

反过来，我们可以使用 `docker import` 命令导入一个本地的 `tar` 包作为镜像：
```
& cat my_container.tar  | docker import - testaa:test
sha256:9190a19d1fb7290fd1ddeeaa3aca78d4f199c96e8757f136fb4728a0d5fecba7
& docker images
testaa              test                9190a19d1fb7        8 seconds ago        183.8 MB

& docker import my_container.tar  test:w
sha256:cf7a9bfba4e92a6dfdfbcc908a4fb7af214264e84da792de8460cf2a2ff6b72f

& my_container.tar  | docker import my_container.tar-testImport:test
open my_container.tar-testImport:test: no such file or directory
```
`docker import` 会吧打包的容器导入为一个镜像。  

`import` 表示从标准输入读取容器内容，`res` `tag` 分别代表生成的镜像和标记。  

这里要注意第三种操作，`res` 的命名规范不能用驼峰式命名，`docker` 会识别错误。 

除了导入本地文件系统的 `tar` 包为一个镜像外，我们还可以使用一个 `url` 来导入网络上的容器：
```
$ docker import url res:tag
```
