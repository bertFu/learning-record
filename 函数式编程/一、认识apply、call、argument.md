# 认识apply、call、argument

> `2016.07.19` - 学习《函数式编程》学习笔记

### apply示例
#### apply方法： 
语法：apply([thisObj[,argArray]])<br/> 
定义：应用某一对象的一个方法，用另一个对象替换当前对象。<br/> 
说明： <br/>
如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。<br/> 
如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。<br/>
```
function splat(fun) {
  return function(array) {
    return fun.apply(null, array);
  };
}

var test = splat(function(a, b) {
  return a + b;
})

test([1, 2]) // 3
```
 1、创建一个 `splat` 函数，返回一个 `fun`，将返回的 `fun` 的参数，作为传入的 `fun` 的参数。<br />
 2、执行 `splat` 赋值一个 `fun`，作用是返回 `参数1 + 参数2`。<br />
 3、执行 `test`，传入 `1, 2`，获得结果 `3`。<br />


### call示例
#### call方法: 
语法：call([thisObj[,arg1[, arg2[,   [,.argN]]]]])<br/> 
定义：调用一个对象的一个方法，以另一个对象替换当前对象。 <br/>
说明： <br/>
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。<br/> 
如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 <br/>
```
function unsplat(fun) {
  return function() {
    console.log(arguments.join) // undefined
    console.log(typeof arguments) // object
    var args = Array.prototype.slice.call(arguments);
    console.log(args.join) // undefined
    
    return fun.call(null, args)
  }
}

var test = unsplat(function(array){
  return array.join(' ');
})

test(1, 2, 3) // "1 2 3"
```
 1、创建一个 `unsplat` 函数，返回一个 `fun`，将返回的 `fun` 的参数，作为传入的 `fun` 的参数（已数组的形式，这里用到 `arguments` 很奇妙的一个属性）。<br />
 2、执行 `unsplat` 赋值一个 `fun`，作用是返回 `参数1 + 参数2`。<br />
 3、执行 `test`，传入 `1, 2`，获得结果 `3`。<br />
 - 作者使用了别的库的东西，使得操作的 `array` 可以调用 `join` 方法。
 - 我只使用 `arguments` 作为参数，没有 `join` 方法。
 - `arguments` 用 `typeof` 打印出来是 `object`，但是表现的形式确实 `[]`。
 - 这个很蛋疼阿...
 - 最后只能通过方法绑定的形式将 `join` 绑到 `arguments` 上实现书上效果。

这两个方法对函数式编程的意义？感觉就是在定义方法的作用域而已，看看作者后面怎么玩吧...