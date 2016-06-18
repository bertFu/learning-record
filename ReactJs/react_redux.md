## react + redux 学习笔记

### PropTypes
```
import React, { PropTypes } from 'react'

//限制组件的props安全
Counter.propTypes = {
  //increment必须为fucntion,且必须存在
  increment: PropTypes.func.isRequired,
  incrementIfOdd: PropTypes.func.isRequired,
  incrementAsync: PropTypes.func.isRequired,
  decrement: PropTypes.func.isRequired,
  //counter必须为数字，且必须存在
  counter: PropTypes.number.isRequired
};
```
上述代码，我们干了几件事：

- 从props中导入变量和方法
- 渲染组件

### actions、reducer、state
```
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'
import Counter from '../components/Counter'
import * as CounterActions from '../actions/counter'

//将state.counter绑定到props的counter
function mapStateToProps(state) {
  return {
    counter: state.counter
  }
}
//将action的所有方法绑定到props上
function mapDispatchToProps(dispatch) {
  return bindActionCreators(CounterActions, dispatch)
}

//通过react-redux提供的connect方法将我们需要的state中的数据和actions中的方法绑定到props上
export default connect(mapStateToProps, mapDispatchToProps)(Counter)
```
state就是数据，组件就是数据的呈现形式，action是动作，action是通过reducer来更新state的。
- 把state的counter值绑定到props上
- 把四个action创建函数绑定到props上

`counter` 内部就可以用到 `actions` 提供的方法，当counter需要做动作的时候，调用 `actions` 的方法，通过 `reducer` 处理后更新 `state`。  
`counter` 通过  `mapStateToProps()` 方法将 `state` 的值绑定到自身的 `props` 对象上，这样就实现了组件与组件之间的数据共享。  
connect这个方法的用法，可以直接看a[pi文档](http://cn.redux.js.org/docs/react-redux/api.html)。

### Action
```
export const INCREMENT_COUNTER = 'INCREMENT_COUNTER'
export const DECREMENT_COUNTER = 'DECREMENT_COUNTER'
//导出加一的方法
export function increment() {
  return {
    type: INCREMENT_COUNTER
  }
}
//导出减一的方法
export function decrement() {
  return {
    type: DECREMENT_COUNTER
  }
}
//导出奇数加一的方法，该方法返回一个方法，包含dispatch和getState两个参数，dispatch用于执行action的方法，getState返回state
export function incrementIfOdd() {
  return (dispatch, getState) => {
    //获取state对象中的counter属性值
    const { counter } = getState()

    //偶数则返回
    if (counter % 2 === 0) {
      return
    }
    //没有返回就执行加一
    dispatch(increment())
  }
}
//导出一个方法,包含一个默认参数delay,返回一个方法,一秒后加一
export function incrementAsync(delay = 1000) {
  return dispatch => {
    setTimeout(() => {
      dispatch(increment())
    }, delay)
  }
}

//这些方法都导出,在其他文件导入时候,使用import * as actions 就可以生成一个actions对象包含所有的export
```

```
### Reducers
import { INCREMENT_COUNTER, DECREMENT_COUNTER } from '../actions/counter'

//reducer其实也是个方法而已,参数是state和action,返回值是新的state
export default function counter(state = 0, action) {
  switch (action.type) {
    case INCREMENT_COUNTER:
      return state + 1
    case DECREMENT_COUNTER:
      return state - 1
    default:
      return state
  }
}
```

### Store
```
import { combineReducers } from 'redux'
import counter from './counter'

//使用redux的combineReducers方法将所有reducer打包起来
const rootReducer = combineReducers({
  counter
})

export default rootReducer
```

reduce()、filter()、map()、some()、every()、...展开属性   这些概念属于es5、es6中的语法，跟react+redux并没有什么联系，我们直接在https://developer.mozilla.org/en-US/ 这里可以搜索到相关api文档。  
[点击学习](http://www.cnblogs.com/lewis617/p/5149006.html)
