# react 脚手架使用教程

## 程序目录

这个项目的结构使用的是 **fractal(不规则碎片形：适合大型项目)***，方法的分组主要是依照特性而不是文件类型。注意，这个目录结构只是一个指引，并不一定要按这个来。这种结构谐在让程序更容易扩展，想了解更多请[点击这里](https://github.com/justingreenberg)。

```
.
├── bin                      # 启动脚本
│   └── compile.js           # 启动命令
│   └── server.js            # server.js服务信息（服务、端口、提示）
├── blueprints               # redux-cli的蓝图
├── build                    # 所有打包配置项
│   └── webpack              # webpack的指定环境配置文件
├── config                   # 项目配置文件
├── server                   # Koa 程序 (使用 webpack 中间件)
│   └── main.js              # 服务端程序入口文件
├── src                      # 程序源文件
│   ├── main.js              # 程序启动和渲染
│   ├── components           # 可复用的直观组件(Presentational Components)
│   ├── containers           # 可复用的容器组件
│   ├── layouts              # 主页结构
│   ├── static               # 静态文件(不要到处imported源文件)
│   ├── styles               # 程序样式
│   ├── store                # Redux指定块
│   │   ├── createStore.js   # 创建和使用redux store
│   │   └── reducers.js      # Reducer注册和注入
│   └── routes               # 主路由和异步分割点
│       ├── index.js         # 用store启动主程序路由
│       ├── Root.js          # 为上下文providers包住组件
│       └── Home             # 不规则路由
│           ├── index.js     # 路由定义和代码异步分割
│           ├── assets       # 组件引入的静态资源
│           ├── components   # 直观React组件
│           ├── container    # 连接actions和store
│           ├── modules      # reducers/constants/actions的集合
│           └── routes **    # 不规则子路由(** 可选择的)
└── tests                    # 单元测试
```

### components

> 可复用组件

1. 组件所以在的目录名字必须大写
2. index.js 只对需要的组件做引入
3. 组件所在的文件名必须大写
