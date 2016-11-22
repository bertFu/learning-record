# 关于this对象

在闭包中使用 this 对象也可能会导致一些问题。我们知道，this 对象是在运行时基于函数的执行环境绑定的；在全局函数中， this 等于 window，而当函数被作为某个对象的方法调用时，this 等于那个对象。不过，匿名函数的执行环境具有全局性，因此 this 对象通常指向 window。但有时候由于编写闭包的方式不同，这一点可能不会那么明显。

> 在通过 call() 或 apply() 改变函数执行环境的情况下，this 就会指向其他对象。

```javascript
var name = "The Window";

var object = {
    name: "My Object",

    getNameFunc: function() {
        return function() {
            return this.name;
        }
    }
}

alert(object.getNameFunc()()); // The Window
```

上例代码先创建了一个全局变量 name，又创建了一个函数 name 属性的对象。

这个对象还包含一个方法 ---- getNameFunc() 返回一个函数，因此调用返回 object.getNameFunc()() 就会立即调用它返回的函数，结果就是返回一个字符串。

这个库组防御的祝福语出是 "The Window" ，即全局 name 变量的值。

每个函数在被调用时，其活动对象都会自动取得两个特殊变量；this 和 arguments。

内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量。

把外部作用域中的 this 对象保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了：

```javascript
var name = "The Window";

var object = {
    name: "My Object",

    getNameFunc: function() {
        var that = this;
        return function() {
            return that.name;
        }
    }
}

alert(object.getNameFunc()()); // My Object
```

在定义匿名函数之前，把 this 对象赋值给了一个名叫 that 的变量。

而在定义了闭包之后，闭包也可以访问这个变量，因为它是我们在包含函数中特意声明的一个变量。

即使在函数返回之后 that 也仍然引用着 object， 所以调用 object.getNameFunc()() 就返回了 "My Object"

> this 和 arguments 也存在同样的问题。如果想访问作用域中的 arguments 对象，必须将对该对象的引用保存到另一个闭包能够访问的变量中。

在几种特殊情况下，this 的值可能会意外地改变：

```javascript
var name = "The Window";

var object = {
    name: "My Object",

    getName: function() {        
        return this.name;
    }
}

object.getName(); // "My Object"
(object.getName)(); // "My Object"
(object.getName = object.getName)(); // "The Window"
```

这里的 getName() 方法只简单地返回 this.name 的值。

`object.getName()` 跟平常一样调用了 object.getName() ， 返回的是 "My Object" ，因为 this.name 就是 object.name。

`(object.getName)()` 代码在调用这个方法前先给它加上了括号。虽然加上括号后，就好像只是在引用一个函数，但this的值得到了维持，因为 object.getName 和 (object.getName) 的定义是相同的。

`(object.getName = object.getName)()` 先执行了一条赋值语句，然后在调用赋值后的结果。因为这个赋值表达式的值是函数本身，所以this的值不能得到维持，结果就返回了 "The Window"。
