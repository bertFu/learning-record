# 引用类型 Object 和 Array

引用类型与传统面向对象程序设计中的类相似，但实现不同。

> 对象在 JavaScript 中被称为引用类型的值，而且有一些内置的引用类型可以用来创建特定的对象。

## Object

> Object 是一个基础类型，其他所有类型都从 Object 继承了基础行为。

```javascript
var person = new Person();
person.name = "Bert";
person.age = 29;

var person = {
  name: "Bert",
  age: 29
}
```

两种创建对象的方式是等价的。

```javascript
var person = {
  name: "Bert",
  age: 29,
  5: true
}
```

这段代码创建的对象：`person` 中含有三个属性：name、age和5.这里的5会被自动转换为字符串。

```javascript
// 下面两种访问对象属性的方法没有任何区别
alert(person["name"]);  // Bert
alert(person.name); // Bert

// 使用括号语法的有点是可以通过变量来访问属性
var propertyName = "name";
alert(person[propertyName]);  // Bert

// 属性名中包含特殊字符时无法使用 '.' 访问，这时候可以使用 '[]' 来访问或赋值
person["first name"] = "Bert"
```

## Array

> Array 类型是一组值的有序列表，同时还提供了操作和转换这些值的功能。

```javascript
// 创建一个数组，长度为0
var colors = new Array();
var colors = [];

// 创建一个数组，长度为2
var colors = new Array(2);
var colors = [, ,]; // 这种方式会有兼容问题

// 创建一个数组，内容为 ["red", "blue", "green"]
var colors = new Array("red", "blue", "green");
var colors = ["red", "blue", "green"];
```

### length 使用小技巧

> length 表示数组的长度，会始终返回0或更大的值。

数据的 `length` 属性不是只读的。因此，通过设置这个属性，可以从数组的末尾移除或想数组末尾添加新项：

```javascript
var colors = ["red", "blue", "green"];
colors.length = 2;
alert(colors[2]); // undefined
```

colors 数组一开始有三个值，将 length 的长度设为2会移除最后一项，再访问 colors[2] 时，原本的 'green' 被移除了，输出 undefined。

```javascript
var colors = ["red", "blue", "green"];
colors.length = 4;
alert(colors[4]); // undefined
```

同理将数组的长度设置为4，超出的位置的值默认会设置为 undefined。

```javascript
var colors = ["red", "blue", "green"];
colors[colors.length] = "black";
colors[colors.length] = "brown";
alert(colors); // ["red", "blue", "green", "black", "brown"]
```

数组的最后一项的下标为 `length-1` ，因此对 `length` 下标赋值数组会在最后一个位置新增对应值。

### Array 的常用方法

- toLocaleString()： 默认情况下会以逗号分隔的字符串的形式返回数组项，需要特别处理数组表现形式时使用。
- toString()： 默认情况下会以逗号分隔的字符串的形式返回数组项，使用输出语句时会默认调用 toString 方法。
- valueOf()： 默认情况下会以逗号分隔的字符串的形式返回数组项，使用判断语句时会默认调用 valueOf 方法。
- join(Str)：不传参数或传入 undefined 会默认使用逗号作为分隔符输出，传入参数则以参数作为分隔符输出。

  ```javascript
  var colors = ["red", "black"];
  alert(colors.join()); // red,black
  alert(colors.join("-")); // red-black
  ```

- push()：接受任意数量的参数，把他们逐个添加到数组末尾，并返回修改后的数组的长度。

  ```javascript
  var colors = new Array();
  var count = colors.push("red", "black");
  alert(count); // 2
  ```

- pop()：从数组末尾移除最后一项，并返回移除的项。

  ```javascript
  var colors = ["red", "black", "blue"];
  var item = colors.pop();
  alert(item); // "blue"
  alert(colors.length); // 2
  ```

- shift()：移除数组中的第一项并返回该项。

  ```javascript
  var colors = ["red", "black", "blue"];
  var item = colors.shift();
  alert(item); // "red"
  alert(colors.length); // 2
  ```

- unshift()：在数组前端添加任意个项并返回新数组的长度。

  ```javascript
  var colors = new Array("red", "black");
  var count = colors.unshift("blue", "green");
  alert(count); // 4
  ```

- reverse()：反转数组项的顺序。

  ```javascript
  var values = [1, 2, 3, 4, 5];
  values.reverse();
  alert(values); // 5,4,3,2,1
  ```

- sort()：默认按字符串升序排列，该方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面。

  - 比较函数接收两个参数。

    - 如果第一个参数应该位于第二个之前则返回一个负数。
    - 如果两个参数相等则返回0。
    - 如果第一个参数应该位于第二个之后则返回一个正数。

  ```javascript
  var values = [0, 1, 5, 10, 15];
  values.sort()
  alert(values); // 0,1,10,15,5
  ```

  sort 方法默认是已字符串进行比较

  ```javascript
  function compare(value1, value2) {
    if (value1 < value2) {
      return -1;
    } else if (value1 > value2) {
      return 1;
    } else {
      return 0;
    }
  }

  var values = [0, 1, 5, 10, 15];
  values.sort(compare);
  alert(values); // 0,1,5,10,15
  ```

- concat()：可以基于当前数组中的所有项创建一个新数组，这个方法会先创建一个副本，然后将接受到的参数添加到这个副本的末尾，最后返回新构建的数组。

  - 参数是一个或多个数组，则该方法会将这些数组中的每一项都添加到结果数组中。
  - 如果传递的值不是数组，这些值就会被简单的添加到结果数组的末尾。

  ```javascript
  var colors = ["red", "blue", "green"];
  var colors2 = colors.concat("yellow", ["black"], "brown")
  alert(colors); // red,blue,green
  alert(colors2); // red,blue,green,yellow,black,brown
  ```

- slice()：基于当前数组中的一或多个项创建一个新数组（不会影响到原数组）。

  - 该方法可以接受一个或两个参数，即要返回项的起始和结束为止。

    - 在只有一个参数的情况下，slice()方法返回从该参数指定为止开始到当前数组末尾的所有项。
    - 如果有两个参数，该方法返回起始和结束位置之间的项 ---- 但不包括结束位置的项。

  ```javascript
  var color = ["red", "green", "blue", "yellow", "purple"];
  var color2 = colors.slice(1);
  var color3 = colors.slice(1, 4);
  alert(colors2); // green,blue,yellow,purple
  alert(colors3); // green,blue,yellow
  ```

  > 如果 slice() 方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。例如，在一个包含5项的数组上调用 slice(-2, -1) 与调用 slice(3, 4) 的到的结果相同。如果结束位置小于起始位置，则返回空数组。

- splice()：数组中最牛逼的方法，他的主要用途是向数组的中部插入项。

  - 删除：可以删除任意数量的项，只需要指定两个参数：需要删除的第一项的位置和要删除的项数。例如：`splice(0, 2)`会删除数组中的前两个项。
  - 插入：可以向制定位置插入任意数量的项，只需要提供3个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。

    - 例如：`splice(2, 0, "red", "green")` 会从当前数组的位置2开始插入字符串 "red" 和 "green"。

  - 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需要指定3个参数：其实位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。

    - 例如：`splice(2, 1, "red", "green")`会删除单签数组位置2的项，然后再从位置2开始插入字符串"red"， "green"。

  - splice() 方法始终会返回一个数组，该数组中包含从原始数组中删除的项（如果没有删除任何项，则返回一个空数组）。

  ```javascript
  var color = ["red", "green", "blue"];

  // 删除第一项
  var removed = colors.splice(0, 1);
  alert(colors); // green, blue
  alert(removed); // red, 返回的数组中只包含一项

  removed = colors.splice(1, 0, "yellow", "orange"); // 从位置1开始插入两项
  alert(colors); // green, yellow, orange, blue
  alert(removed); // []

  removed = colors.splice((1, 1, "red", "purple"))
  alert(colors); // green, red, purple, orange, blue
  alert(removed); // yellow
  ```

- indexOf()：从头开始向末尾查找，接受两个参数：要查找的项和（可选的）表示查找启动位置的索引。

  - 没有找到返回 -1。
  - 找到返回数组的下标。
  - 查找的项必须严格相等（===）。

- lastIndexOf()：从末尾开始向前查找，接受两个参数：要查找的项和（可选的）表示查找启动位置的索引。

  - 没有找到返回 -1。
  - 找到返回数组的下标。
  - 查找的项必须严格相等（===）。

```javascript
var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];

// 传入一个参数，表示查找的项
alert(numbers.indexOf(4)) // 3
alert(numbers.lastIndexOf(4)) // 5

// 传入两个参数
alert(numbers.indexOf(4, 4)) // 5
alert(numbers.lastIndexOf(4, 4)) // 3

// 查询对象
var person = {name: "Bert"};
var people = [{name: "Bert"}];
var morePeople = [person];
alert(people.indexOf(person)) // -1
alert(morePeople.indexOf(person)) // 0
```

- every()：遍历数组，如果传入的函数每一项都返回 true 该方法会返回true，如果有一项返回false，则该方法返回 false（数组每项的 && 的关系）。
- some()：遍历数组，如果传入的函数每一项都返回 false 该方法会返回 false，如果有一项返回 true，则该方法返回 true （数组每项的 || 的关系）。

  ```javascript
  var numbers = [1,2,3,4,5,4,3,2,1];
  var everyResult = numbers.every(function(item. index, array) {
    return (item > 2);
  })
  alert(everyResult) // false

  var someResult = numbers.some(function(item, index, array) {
    return (item > 2);
  })
  alert(someResult); // true
  ```

- filter()：遍历数组，将传入的函数的返回 true 的项值重新生成新数组，过滤返回 false 的项（数组的过滤器，用于搜索）。

  ```javascript
  var numbers = [1,2,3,4,5,4,3,2,1];
  var filterResult = numbers.filter(function(item, index, array) {
    return (item > 2);
  })
  alert(filterResult); // [3,4,5,4,3]
  ```

- forEach()：针对数组的每一项运算传入的函数，没有返回值。

- map()：遍历数组，将传入的函数的返回值作为新的数组的值。

  ```javascript
  var numbers = [1,2,3,4,5,4,3,2,1];
  var mapResult = numbers.map(function(item, index, array) {
    return item * 2;
  })

  alert(mapResult); // [2,4,6,8,10,8,6,4,2]
  ```

- reducer()：方法可以执行求数组中所有值之和的操作，会将前一次返回的结果作为下一次运算的第一个参数（数组的递归运算）。

  ```javascript
  var values = [1,2,3,4,5];
  var sum = values.reduce(function(prev, cur, index, array) {
    return prev + cur;
  })
  alert(sum); //15
  ```

- reducerRight()：与 reducer 作用类似，只不过方向相反（数组的反向递归）。

  ```javascript
  var values = [1,2,3,4,5];
  var sum = values.reduceRight(function(prev, cur, index, array) {
    return prev + cur;
  })
  alert(sum); //15
  ```
