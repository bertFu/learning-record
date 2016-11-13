# Redux 使用笔记

用 Redux 也有小半年了，总结下自己的心路历程

## 项目结构

### 示例1

```
.
├── dist                              # 所有打包配置项
├── src                               # 程序源文件
│   ├── actions                       # actions 管理
│   ├── api                           # superagent 处理，基于node的Ajax组件
│   ├── components                    # 可复用的直观组件(Presentational Components)
│   ├── constants                     # 常量管理
│   ├── reducers                      # reducers 管理
│   ├── routes                        # 主路由和异步分割点
│   │   └── index.js                  # 用store启动主程序路由
│   ├── static                        # 静态资源文件
│   ├── store                         # Redux指定块
│   │   ├── middlewares               # 中间件管理
│   │   │   ├── afterApiMiddleware.js # 处理用户登录中间件
│   │   │   └── promiseMiddleware.js  # 处理 Pormise 中间件
│   │   ├── createStore.js            # 创建和使用redux store
│   │   ├── reducers.js               # Reducer注册和注入
│   │   └── types.js                  # 管理 Action、Reducer 公用的 types
│   │── util                          # 工具包
│   │── views                         # 业务页面管理
│   │   └── Home                      # 不规则路由
│   │       ├── Home.js               # 将组件集成成为业务模块
│   │       ├── Home.less             # 业务模块对应的css
│   │       └── index.js              # 导入业务模块，使用 redux 将模块需要的 porps 传入
│   ├── index.htlm                    # 主入口 HTML 文件
│   ├── index.js                      # 主要 JS 文件
│   └── index.less                    # 主入口 css 文件
└── tests                             # 测试
```

这个目录结构是现在正在使用的，也比较顺手的接口：

1. `action`：在（views）文件下的组件需要使用时，根据目录接口导入 `action` 即可；
2. `reducer`：记住，在新增 `reducer` 的时候一定要在 `createStore.js` 中导入（经常碰到一件很尴尬的事情，写了 reducer 为什么不会触发呢....）；
3. `util`：放着一些工具，如时间格式化，通用验证，方法判断...
4. `api`：提供 `Ajax` 工具，目前使用的是 `superagent` 如果喜欢可以换成 `reqwest` 等...
5. `components`：通用组件；
6. `views`：业务组件；
7. `store`：核心包，直接和路口 `index.js` 挂钩，很少会动到里面的东西。

### 示例2

```
.
├── dist                              # 所有打包配置项
├── src                               # 程序源文件
│   ├── actions                       # actions 管理
│   ├── api                           # superagent 处理，基于node的Ajax组件
│   ├── components                    # 可复用的直观组件(Presentational Components)
│   ├── constants                     # 常量管理
│   ├── reducers                      # reducers 管理
│   ├── routes                        # 主路由和异步分割点
│   │   └── index.js                  # 用store启动主程序路由
│   ├── static                        # 静态资源文件
│   ├── store                         # Redux指定块
│   │   ├── middlewares               # 中间件管理
│   │   │   ├── afterApiMiddleware.js # 处理用户登录中间件
│   │   │   └── promiseMiddleware.js  # 处理 Pormise 中间件
│   │   ├── modules                   # 每个业务对应的Action
│   │   │   └── menu
│   │   │       ├── action.js         
│   │   │       └── reducer.js        
│   │   ├── createStore.js            # 创建和使用redux store
│   │   ├── reducers.js               # Reducer注册和注入
│   │   └── types.js                  # 管理 Action、Reducer 公用的 types
│   │── util                          # 工具包
│   │── views                         # 业务页面管理
│   │   └── Home                      # 不规则路由
│   │       ├── Home.js               # 将组件集成成为业务模块
│   │       ├── Home.less             # 业务模块对应的css
│   │       └── index.js              # 导入业务模块，使用 redux 将模块需要的 porps 传入
│   ├── index.htlm                    # 主入口 HTML 文件
│   ├── index.js                      # 主要 JS 文件
│   └── index.less                    # 主入口 css 文件
└── tests
```

主要区别是将 `action、reducer` 放在 `store/modules` 目录下，以业务模块划分，使用起来不是很顺手。

初期搭建的项目是将 `action、reducer、state` 放在 `store` 的 `module` 目录下，最开始的理解是，每个模块都应该有自己独立的 `action、reducer、state`，但因为对 `react` 和 `redux` 的理解太浅，导致整个项目维护成本巨大，各个组件嵌套 `props` 传递，各种交集，新需求下来经常牵一发动全身。

### 示例3

```
.
├── bin                      # 启动脚本
├── blueprints               # redux-cli的蓝图
├── build                    # 所有打包配置项
│   └── webpack              # webpack的指定环境配置文件
├── config                   # 项目配置文件
├── server                   # Express 程序 (使用 webpack 中间件)
│   └── main.js              # 服务端程序入口文件
├── src                      # 程序源文件
│   ├── main.js              # 程序启动和渲染
│   ├── components           # 全局可复用的表现组件(Presentational Components)
│   ├── containers           # 全局可复用的容器组件
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

主要区别是将 `action、reducer` 放在 `组件/container` 目录下，也是以业务模块划分，作者说这是大型项目的目录设计，初学者领悟不透，不好用，所以也没有在项目中使用这种结构。

## Action使用

Action 在我现在的理解他的用处是：数据获取（调接口、从组建得到数据），数据过滤，数据分发。

Action的设计在我认为有几个模式：初始化、销毁、增、删、查、改、修改State。Action名字一定要通俗易懂，在组件中调用才会知道这个是干什么的，提高代码可读性。

```javascript
export function getList() {
    return (dispatch, getState) => {
        const params = {}

        dispatch({
            type: types.GET_DEMO_LIST,
            payload: {
                promise: get_demo_list(params)
            }
        })
    }
}
export function setTitle(title) {
    return (dispatch, getState) => {

        dispatch(setState({
            title: title
        }))
    }
}
function setState(state) {
    return (dispatch, getState) => {
        dispatch({
            type: types.SET_DEMO_STATE,
            params: {
                state: state
            }
        })
    }
}
```

上例举列表获取和title设置的例子，title设置这里还有奇异，其实只要暴露 `setState()` 方法即可，这块看开发者自己喜好吧。

这里使用了 dispatch、getState 这个两个功能，我认为这两个就是Action的神技，State的获取简单多了，不用传来传去，弄的各种Bug，一个Aciton中可以处理多件事，宝宝再也不用当心异步处理了，巨牛逼！！！

## Reducer使用

Reducer 的设计在 原文：[Reducer](http://cn.redux.js.org/docs/basics/Reducers.html) 中写的很好了，对于初学者而言，看到这些东西就望而生畏了...

- 为什么我写的 Reducer 总是无法设计的让自己满意？
- 为什么感觉我写的 Reducer 难以维护？
- 我感觉已经写的很通用了，但是业务一变，就炸锅了...

有一天我终于想通了，一开始就能把 `Reducer` 设计好的，一定要对业务有非常深的了解，深到什么程度？这个产品，你想怎么变都在我的预料之中，无非就是这么1,2,3,4,5随你来，一般都是在一个行业里滚了好久的大牛，还有一种就是天才，随便写随便适配，你来什么我挡什么，虽然我开始没有想到这样的需求，当时我随手写出来的代码适应性就是强。

反正我知道我不是天才，我也不是资深老玩家，那么要怎么去整这个 `Reducer` 呢?在最开始的时候，把自己的能想到的方案都想一遍（想一定比做的快），然后有能力就把所有想法记录下来（记录想法也肯定比开发来的高效），最后如果无法确定自己的想法那个是最好的，那就选择最容易，开发效率最高的来。

为什么这么做？因为我试过，在我把能想的都想了，写下来了，花了很多时间去对比哪种好，结果选择了一个相对复杂的实现方式，然后还想当然的觉得自己好牛逼呀!!最后业务部门来新需求了，麻痹，完全和之前的需求不在一个维度上，为什么这么说？因为和我之前思考的东西都不相关，然后要推到重来，那个工作量，说多了都是泪，默默加班了好久（后来我知道了莫装逼...）

这时候问题又来了，如果只写最简单的为什么还要思考这么多呢？如果你是为了工作而工作的人，那么随便你把，反正能完成任务就好，工作量多了，就干呗，反正有钱赚。如果你是有一点点想提升自己的，把自己能想的都想一遍，当新需求出现的时候，符合自己原来的某些想法，那证明了自己有远见呀，把想过的东西实现起来切换成本和效率都不是盖的；如果这个需求和逾期的完全不一样，那也证明了自己还有提升的空间呀，前提是这个不是无厘头的需求（真正好的需求是可以带动开发成长的）。

说了这么多，总结下现在的做法：

```javascript
const InitState = {
    list: [{
        id: 1,
        title: '测试'
    }],
    loading: false,
    pager: {
        pageID: 1,
        pageSize: 10,
        total: 0,
    },

    title: '',

    updDemoInfo: {
        title: '',
    },
    oldUpdDemoInfo: {
        title: '',
    },
}

export default createReducer(InitState, {
    // 删除 Demo
    [`${types.DEL_DEMO}_PENDING`]: (state, data, params) => {
        let list = state.list;

        list = state.list.map((item) => {
            if (item.id == params.id) {
                item.delStatus = 'loading';
            }
            return item;
        })

        return Object.assign({}, state, {
            list: list,
        })
    },
    [`${types.DEL_DEMO}_SUCCESS`]: (state, data, params) => {
        let list = state.list;

        if (data.code == '0') {
            list = state.list.filter((item) => {
                return item.id != params.id;
            })
        } else {
            list = state.list.map((item) => {
                if (item.id == params.id) {
                    item.delStatus = 'error';
                }
                return item;
            })
        }

        return Object.assign({}, state, {
            list: list,
        })
    },
    [`${types.DEL_DEMO}_ERROR`]: (state, data, params) => {
        let list = state.list;

        list = state.list.map((item) => {
            if (item.id == params.id) {
                item.delStatus = 'error';
            }
            return item;
        })

        return Object.assign({}, state, {
            list: list,
        })
    },
})
```

一个简单的功能无非就是增、删、改、查，`state` 设计围绕这几点，加上 loading，参数，和特定值的存储，基本就ok了。如果你说要把这些在往细的分，那么如果你有办法维护可以分享给我。

我试过将他们分成增、删、改、查，四个模块然后每个模块只干自己的事情，然后就没然后了...完全跪了阿，维护成本巨大，我在做任何操作的时候，要去动到list，都要夸文件的分发 `action` 改代码的时候，谁知道你这个添加功能会碰到那边的list的数据阿？然后各种错综复杂，Bug连出，我都有辞职的冲动了，只能怪自己设计能力不佳。

所以真心希望有人能够和我分享分享他是如何设计 `reducer` 的心得，在使用中得到了哪些便利，感激不尽。

上例是做删除操作，如果服务端返回删除成功，则直接在本地操作list，进行删除，这样可以省去一次列表获取的请求，提高用户体验。

## State设计

State 的设计在上例 Reducer 中有提及到：

```
const InitState = {
    list: [{
        id: 1,
        title: '测试'
    }],
    loading: false,
    pager: {
        pageID: 1,
        pageSize: 10,
        total: 0,
    },

    title: '',

    updDemoInfo: {
        title: '',
    },
    oldUpdDemoInfo: {
        title: '',
    },
}
```

根据业务不同有各种设计，最初级的就是增、删、查、改，加入各种loading，参数，状态，就能满足基本需求了。

## 中间件使用

- thunkMiddleware：非常牛逼的 dispatch 就是通过他提供的。
- createLogger：Action的日志打印，开发时不用你在通过大量的 console 来调试了。
- promiseMiddleware：Promise转换中间件，把三种状态分成3中Action，非常好用。
- afterApiMiddleware：自定义中间件，作为过滤器，处理一些通用逻辑。

## Util

```javascript
export function createReducer(initialparams, reducerMap) {
    return (params = initialparams, action) => {
        const reducer = reducerMap[action.type]

        return reducer ? reducer(params, action.payload ? action.payload : {}, action.params) : params
    }
}

export function createReducer (initialState, reducerMap) {
  return (state = initialState, action) => {
    const reducer = reducerMap[action.type]
    return reducer ? reducer(state, action) : state
  }
}
```

上例是 `Reducer` 生成器, `reducer` 通过 `switch case` 来判断 `action.type` 针对指定的 `type` 构建出新的 `state`，使用这段 js 构建出的 `reducer` 代码减少了 `switch case` 的模板代码，使得 `reducer` 看起来更加清晰。这块可以按自己内部代码结构来设计，并没有硬性要求。

## API

`Ajax` 分装，统一管理请求，在项目开发的时候，发现，因为思路不清晰，很多 `action` 有重复的 `Ajax`，在服务端接口改动的时候，修改接口 `url` 和是一件很痛苦的事情，所以统一管理请求，在 `action` 中调用，便于项目维护。

```javascript
import Api from './api';

const api = new Api({
  baseURI: '/',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json'
  }
})

export default api

export function Lois(data) {
    return api.post('inter/xxx', {data: data})
}
```

这块代码是我们做的一个统一转发的接口，统一放在 `index.js` 文件中，方便导出。

```javascript
export async function add_task(params) {
    return Lois({
        method: 'POST',
        serverName: 'task',
        data: params
    })
}

export async function upd_task(params) {
    return api.put('url', {data: xxx})
}

export async function del_task(params) {
    return reqwest({
        url: 'http://xxx.com',
        method: 'delete',
        type: 'json',
        data: [
          { name: 'sign', value: sign },
          { name: 'ts', value: ts },
          { name: 'appID', value: appID }]
      })
}
```

同样的在该目录下有一个 `service.js` 文件，这个文件有各种业务接口的调用，对外暴露业务名。

我们经常会碰到一个情况，一个或多个 `Action` 中会调用到相同的接口，如果不统一处理，维护起来成本相当高，还很容易出Bug，关键是，Bug出来还不容易找到...

自从把所有对服务端的接口统一处理后，修改起来就方便多了，并且可以在相关的业务接口中加入扩展逻辑，相当之方便。

## Immutable

刚开始对其本身的用法不明确的情况下胡乱使用 `immutable`，导致项目维护时各种问题，反思了下，好的技术只有在需要他并且能够用好它的情况下在整合到项目中，胡乱的添加各种技术，只会把自己推往更深的坑里。

对其Api不熟悉，设置 `state` 真的感觉一个头两个大，经常在 `reducer` 中设置的一个值，然后这个值并没有在 `immutable` 中体现，然后各种报错，开发上消耗了大量的时间，切维护起来非常难受，在 `console` 中打印出来的对象都不知道是什么鬼...

但是不可否认的是这个插件是非常不错的，当我们的项目在性能上要遇到瓶颈时，会选择加入该插件，前提是，想要把他的Api玩溜了，不然怎用怎么坑。

```javascript
return state.set('loading', false)
      .set('sourceType', sourceType)
      .set('isDesc', isDesc)
      .set('startTime', startTime)
      .set('endTime', endTime)

return Object.assign({}, state, {
    loading: false,
    sourceType: sourceType,
    isDesc: isDesc,
    startTime: startTime,
    endTime: endTime,
})
```

上例是使用 `immutable` 与 `Object.assign()` 的对比，看上去没有什么太大的区别， `immutable` 还能提升性能，但是实际使用的时候就会知道不会用 `immutable` 就去使用它的痛苦了。
