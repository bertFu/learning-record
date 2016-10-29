# function

> 函数实际上是一个对象。

> 每个韩式都是 `Function` 类型的实例，而且都与其他引用类型一样具有属性和方法。

> 由于函数式对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。

## 函数定义

```javascript
function sum(num1, num2) {
    return num1 + num2;
}

var sum = function(num1, num2) {
    return num1 + num2;
}

var sum = new Function("num1", "num2", "return num1 + num2") // 不推荐
```

函数名是指向函数的指针，因此函数名与包含对象指针的其他变量没有什么不同。

一个函数可以有多个名字（指针），例如：

```javascript
function sum(num1, num2) {
    return num1 + num2;
}
alert(sum(10, 10)); // 20

var anotherSum = sum;
alert(anotherSum(10, 10)); //20

sum = null;
alert(anotherSum(10, 10)); //20
```

上面例子可以看出，指向函数的变量，实际上是指向函数对象的指针，所以当其中一个赋值为 `null` 时，并不会影响另一个（指针）变量。

## 没有重载（深入理解）

> 将函数名想象为指针，也有助于理解为什么ECMAScript中没有函数重载的概念。

```javascript
function addSomeNumber(num) {
    return num + 100;
}

function addSomeNumber(num) {
    return num + 200;
}

var result = addSomeNumber(100); // 300
```

```javascript
var addSomeNumber = function(num) {
    return num + 100;
}
addSomeNumber = function(num) {
    return num + 200;
}
```

虽然声明了两个函数，结果是后面的函数覆盖了前面的函数。

### 函数声明与函数表达式

> 解析器在向执行环境中加载数据时，对函数声明和函数表达式并非一视同仁。

> 解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）。

> 函数表达式则必须等到解析器执行到它们所在的代码行，才会真正被执行。

```javascript
alert(sum(10, 10)); // 20
function sum(num1, num2){
    return num1 + num2;
}

alert(sum1(10, 10)) // 报错：TypeError: sum1 is not a function(…)
var sum1 = function(num1, num2){
    return num1 + num2;
}
```

除了声明时候可以通过变量访问函数这一点区别之外，函数声明与函数表达式的语法其实是等价的。

> 可以同时使用函数声明和函数表达式，不过这种语法在 `Safari` 中会导致错误。

### 作为值的函数

> 因为函数本身就是变量，所以函数也可以作为值来使用。

> 函数可以像参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。

```javascript
function callSomeFunction(someFunction, someArgument) {
    return someFunction(someArgument);
}

function add10(num) {
    return num + 10;
}

var result1 = callSomeFunction(add10, 10);
alert(result1); // 20

function getGreetine(name) {
    return "Hello, " + name;
}

var result2 = callSomeFunction(getGreetine, "Bert");
alert(result2); // Hello, Bert
```

### 函数内部属性

函数内部有两个特殊对象：arguments 和 this。

#### arguments

`arguments` 是一个数组对象，包含着传入函数中的所有参数。

> 虽然 `arguments` 的主要用途是保存函数参数，但这个对象还有一个名叫 `callee` 的属性，该属性是一个指针，指向拥有这个 `arguments` 对象的函数。

```javascript
function factorial(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * factorial(num -1);
    }
}

factorial(3) // 6

function factorial(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num -1);
    }
}

factorial(5) // 120
```

使用函数名的阶乘如果函数名变了，递归调用的函数的名字也要跟着改变。

但是，如果使用的是 `arguments.callee` 则不管函数名如何变化，内部代码都不用担心会出现错误。

#### this

函数内部的另一个特殊对象，其行为与`Java` 和 `C#` 中的 `this` 大致类似。

`this` 引用是函数据以执行的环境对象 ---- 或者也可以说是 `this` 值（当在网页的全局作用域中调用函数时，`this` 对象引用就是 `window`）。

```javascript
window.color = "red";
var o = { color: "blue" };

function sayColor() {
    alert(this.color);
}

sayColor(); // red

o.sayColor = sayColor;
o.sayColor(); // blue
```

根据作用域的不同， `this` 指针指向的对象也不同。

当在全局作用域中调用 `sayColor()` 时，this引用的是全局对象 `window`；换句话说 `this.color` 求值会转换成对 `window.color` 求值，

当把函数赋给对象 `o` 并调用 `o.sayColor()` 时， `this` 引用的是对象 `o`，因此对 `this.color` 求值会转换成对 `o.color` 求值，结果就返回了 `bule`。

> 注意：函数的名字仅仅是一个包含指针的变量而已。因此，即使是在不同的环境中执行，全局的 `sayColor()` 函数与 `o.sayColor()` 指向的仍然是同一个函数
