# 全局作用域

> BOM 的核心对象是 window， 它表示浏览器的一个实例。在浏览器中，window 对象由双重角色，它既是通过 JavaScript 访问浏览器窗口的一个接口，又是通过 ECMAScript 规定的 Global 对象。这意味着在网页中定义的任何一个对象、变量和函数，都以 window 对位其 Global 对象，因此有权访问 parseInt() 等方法。

由于 window 对象同时扮演者 ECMAScript 中 Global 对象的角色，因此所有在全局走用于中声明的变量、函数都会变成 window 对象的属性和方法。

```javascript
var age = 29;

function sayAge() {
    alert(this.age);
}

alert(window.age); // 29
sayAge(); // 29
window.sayAge(); // 29
```

我们在全局作用域中定义了一个变量 age 和一个函数 sayAge()，它们被自定归在了 window 对象名下。于是，可以通过 window.age 访问变量 age，可以通过 window.sayAge() 访问函数 sayAge()。由于 sayAge() 存在于全局作用域中，因此 this.age 被映射到 window.age ，最终显示的任然是正确的结果。

抛开全局变量会成为 window 对象的属性不谈，定义全局变量与在 window 对象上直接定义属性还是有一点差别：全局变量不能通过 delete 操作符删除，而直接在 window对象上的定义属性可以。

```javascript
var age = 29;
window.color = "red";

// 在 IE<9 时抛出错误，在其他所有浏览器中都返回 false
delete window.age;

// 在 IE<9 时抛出错误，在其他所有浏览器中都返回 true
delete window.color;

alert(window.age); // 29
alert(window.color); // undefined
```

刚才使用 var 语句添加的 window 属性有一个名为 [[Configurable]] 的特性，这个特性的值被设置为 false，因此这样定义的属性不可以通过 delete 操作符删除。

IE8及更早版本在遇到使用 delete 删除 window 属性的语句时，不管该属性最初是如何创建的，都会抛出错误，以示警告。IE9及更高版本不会抛出错误。

另外，还要记住一件事：尝试访问未声明的变量会抛出错误，但是通过查询 window 对象，可以指定某个可能未声明的变量是否存在。

```javascript
// 这里会抛出错误，因为 oldValue 未定义
var newValue = oldValue;

// 这里不会抛出错误，因为这是一次属性查询
// newValue 的值是 undefined
var newValue = window.oldValue;
```

> windows.Mobile 平台的IE浏览器不允许通过 window.prototype = value 之类的形式，直接在 window 对象上创建新的属性或方法。可是，在全局作用域中声明的所有变量和函数，照样会变成 window 对象的成员。
