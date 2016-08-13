# NPM 命令学习笔记

## NPM 版本管理

查看 `npm` 版本号

```
npm -v
```

如果你安装的是旧版本的 `npm`，可以通过 `npm` 命令来升级

```
sudo npm install npm -g
```

`npm` 安装 `Node.js` 模块

```
npm install <Module Name>
```

使用以下命令来查看所有全局安装的模块

```
npm ls -g
```

查看 `node_modules` 目录下的模块

```
npm ls
```

## 卸载操作

卸载 Node.js 模块

```
npm uninstall express
```

更新模块

```
npm update express
```

搜索模块

```
npm search express
```

## 镜像设置

查看镜像源

```
npm config list
```

查看配置

```
npm -l
```

设置淘宝镜像

```
npm config set registry " https://registry.npm.taobao.org "
```
