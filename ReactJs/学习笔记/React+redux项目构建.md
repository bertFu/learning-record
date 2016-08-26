# React+redux+webpack 项目构建：初具规模

## 目录结构

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

### Actions、Reducers

初期搭建的项目是将 `action、reducer、state` 放在 `store` 的 `module` 目录下，最开始的理解是，每个模块都应该有自己独立的 `action、reducer、state`，但因为对 `react` 和 `redux` 的理解太浅，导致整个项目维护成本巨大，各个组件嵌套 `props` 传递，各种交集，新需求下来经常牵一发动全身。

而现在将所有的 `action` 统一放在 `actions` 中进行管理，业务组件在需要的时候加载对应的 `action` 方法， `reducer` 监听对应的 `action` 下发新的 `state`。

这种较为统一的 `redux` 管理中，学习切分 `state`，降低 `redux` 和 `react` 组件的耦合，维护起来思路比较清晰。

### store/types

统一管理 `action` 的 `type`，避免重复开发 `action`。

### api

`Ajax` 分装，统一管理请求，在项目开发的时候，发现，因为思路不清晰，很多 `action` 有重复的 `Ajax`，在服务端接口改动的时候，修改接口 `url` 和是一件很痛苦的事情，所以统一管理请求，在 `action` 中调用，便于项目维护。

### components、views

区分普通组件和业务组件，业务组件整合需要的普通组件，通过 `redux` 管理 `props`，使得组件逻辑更加清晰。

### Immutable

刚开始对其本身的用法不明确的情况下胡乱使用 `immutable`，导致项目维护时各种问题，反思了下，好的技术只有在需要他并且能够用好它的情况下在整合到项目中，胡乱的添加各种技术，只会把自己推往更深的坑里。

### createReducer

```javascript
// reducer生成器
export function createReducer (initialState, reducerMap) {
  return (state = initialState, action) => {
    const reducer = reducerMap[action.type]
    return reducer ? reducer(state, action) : state
  }
}
```

`reducer` 通过 `switch case` 来判断 `action.type` 针对指定的 `type` 构建出新的 `state`，使用这段 js 构建出的 `reducer` 代码减少了 `switch case` 的模板代码，使得 `reducer` 看起来更加清晰。

## middlewares

中间件是 `redux` 针对 `flux` 不足，产出的一个很有趣的工具，如何用好它是一门艺术，可以添加各种 `filter middlewares` 来使得整体功能变得更强大。

`Redux` 提供了 `applyMiddleware(...middlewares)` 来将中间件应用到 `createStore`。`applyMiddleware` 会返回一个函数，该函数接收原来的 `creatStore` 作为参数，返回一个应用了 `middlewares` 的增强后的 `creatStore`。

```javascript
export default function applyMiddleware(...middlewares) {
  return (createStore) => (reducer, preloadedState, enhancer) => {
    //接收createStore参数
    var store = createStore(reducer, preloadedState, enhancer)
    var dispatch = store.dispatch
    var chain = []

    //传递给中间件的参数
    var middlewareAPI = {
      getState: store.getState,
      dispatch: (action) => dispatch(action)
    }

    //注册中间件调用链
    chain = middlewares.map(middleware => middleware(middlewareAPI))
    dispatch = compose(...chain)(store.dispatch)

    //返回经middlewares增强后的createStore
    return {
      ...store,
      dispatch
    }
  }
}
```

### redux-thunk

处理异步 `action` 的中间件，

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

可以看出来， `thunk` 处理异步的逻辑就是判断返回的 `action` 是否是 `function`，来执行 `function`，或者执行 `action` 。

### redux-logger

日志打印中间件

```javascript
export default function({getState,dispatch}) {
    return (next) => (action) => {
        console.log('pre state', getState());
        // 调用 middleware 链中下一个 middleware 的 dispatch。
        next(action);
        console.log('after dispatch', getState());
    }
}
```

日志打印其实就是在 `action` 执行前后分别打印需要的信息，观测每个 `action` 的变化对发开的帮助是很大的。

### Promise

处理 `action` 中的 Promise

```javascript
action.payload.then(
    result => dispatch({ ...action, payload: result }),
    error => {
        dispatch({ ...action, payload: error, error: true });
        return Promise.reject(error);
    }
)
```

代码逻辑也很简单， 在 `action` 的 `payload` 参数中指定一个 `Promise`，中间件判断如果是 `Promise` 则对相应的 `result` 和 `error` 发送 `dispatch` 来达到效果。

## State 维护

如何维护 `state` 一直是困扰我的一大难题，要分的多细，如何划分组件的 `state`，如何使用 `reducer` 更新 `state`，玩了快3个月的 `react` 仍然一头雾水。

> `State` 的设计原则

> 1. 是否是从父级通过 `props` 传入的？如果是，可能不是 `state` 。

>   - 可以通过外部传入的数据，不应该使用 `state` 管理

> 2. 是否会随着时间改变？如果不是，可能不是 `state` 。

>   - 不会改变的数据，不应该使用 `state` 管理

> 3. 能根据组件中其它 `state` 数据或者 `props` 计算出来吗？如果是，就不是 `state` 。

>   - 能够通过 `props` 计算出来的不应该使用 `state` 管理

> 对于应用中的每一个 state 数据：

> - 找出每一个基于那个 state 渲染界面的组件。
> - 找出共同的祖先组件（某个单个的组件，在组件树中位于需要这个 state 的所有组件的上面）。
> - 要么是共同的祖先组件，要么是另外一个在组件树中位于更高层级的组件应该拥有这个 state 。
> - 如果找不出拥有这个 state 数据模型的合适的组件，创建一个新的组件来维护这个 state ，然后添加到组件树中，层级位于所有共同拥有者组件的上面。

可以看出 `state` 的设计原则是不可预测的变量，但是如何构建 `state` 树呢？

1. 需要对业务深入了解
2. 要有一定的设计基础

胡乱的设计，到最后会发现，只是在给自己埋坑...

## 拆分 Reducer

原文：[Reducer](http://cn.redux.js.org/docs/basics/Reducers.html)

```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        todos: state.todos.map((todo, index) => {
          if(index === action.index) {
            return Object.assign({}, todo, {
              completed: !todo.completed
            })
          }
          return todo
        })
      })
    default:
      return state
  }
}
```

这里的 todos 和 visibilityFilter 的更新看起来是相互独立的。有时 state 中的字段是相互依赖的，需要认真考虑，但在这个案例中我们可以把 todos 更新的业务逻辑拆分到一个单独的函数里：

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        todos: todos(state.todos, action)
      })
    default:
      return state
  }
}
```

注意 todos 依旧接收 state，但它变成了一个数组！现在 todoApp 只把需要更新的一部分 state 传给 todos 函数，todos 函数自己确定如何更新这部分数据。这就是所谓的 reducer 合成，它是开发 Redux 应用最基础的模式。

> 在目前无法达到准确的分化 `state` 时，先将 `reducer` 对于 `state` 的操作放在一起，然后在对业务有更深刻的理解时，慢慢的分化，来达到 `state` 维护的目的

下面深入探讨一下如何做 reducer 合成。能否抽出一个 reducer 来专门管理 visibilityFilter？当然可以：

```javascript
function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return action.filter
  default:
    return state
  }
}
```

现在我们可以开发一个函数来做为主 reducer，它调用多个子 reducer 分别处理 state 中的一部分数据，然后再把这些数据合成一个大的单一对象。主 reducer 并不需要设置初始化时完整的 state。初始时，如果传入 undefined, 子 reducer 将负责返回它们的默认值。

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}

function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

注意每个 reducer 只负责管理全局 state 中它负责的一部分。每个 reducer 的 state 参数都不同，分别对应它管理的那部分 state 数据。

现在看过起来好多了！随着应用的膨胀，我们还可以将拆分后的 reducer 放到不同的文件中, 以保持其独立性并用于专门处理不同的数据域。

最后，Redux 提供了 combineReducers() 工具类来做上面 todoApp 做的事情，这样就能消灭一些样板代码了。有了它，可以这样重构 todoApp：

```javascript
import { combineReducers } from 'redux';

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp;
```

注意上面的写法和下面完全等价：

```javascript
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

> 这块代码让我更加确定，先统一管理 `reducer` 后在慢慢分化，原先为了分化而分化，坑苦了自己

你也可以给它们设置不同的 key，或者调用不同的函数。下面两种合成 reducer 方法完全等价：

```javascript
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})
```

```javascript
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}
```

> 持续更新，下一篇 `webpack` 的使用笔记
