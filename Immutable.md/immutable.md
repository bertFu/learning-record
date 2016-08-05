### 介绍

[官方文档](http://facebook.github.io/immutable-js/docs/#/)
[Immutable 详解及 React 中实践](http://www.w3ctech.com/topic/1595)

有人说 Immutable 可以给 React 应用带来数十倍的提升，也有人说 Immutable 的引入是近期 JavaScript 中伟大的发明，因为同期 React 太火，它的光芒被掩盖了。这些至少说明 Immutable 是很有价值的，下面我们来一探究竟。    
  
JavaScript 中的对象一般是可变的（Mutable），因为使用了引用赋值，新的对象简单的引用了原始对象，改变新的对象将影响到原始对象。如 foo={a: 1}; bar=foo; bar.a=2 你会发现此时 foo.a 也被改成了 2。虽然这样做可以节约内存，但当应用复杂后，这就造成了非常大的隐患，Mutable 带来的优点变得得不偿失。为了解决这个问题，一般的做法是使用 shallowCopy（浅拷贝）或 deepCopy（深拷贝）来避免被修改，但这样做造成了 CPU 和内存的浪费。

Immutable 优点:
1. Immutable 降低了 Mutable 带来的复杂度，可变（Mutable）数据耦合了 Time 和 Value 的概念，造成了数据很难被回溯。
2. 节省内存:
3. Undo/Redo，Copy/Paste，甚至时间旅行这些功能做起来小菜一碟
4. 并发安全
5. 拥抱函数式编程

Immutable 缺点：
1. 需要学习新的 API
2. 增加了资源文件大小
3. 容易与原生对象混淆
```
这点是我们使用 Immutable.js 过程中遇到最大的问题。写代码要做思维上的转变。

虽然 Immutable.js 尽量尝试把 API 设计的原生对象类似，有的时候还是很难区别到底是 Immutable 对象还是原生对象，容易混淆操作。

Immutable 中的 Map 和 List 虽对应原生 Object 和 Array，但操作非常不同，比如你要用 map.get('key') 而不是 map.key，array.get(0) 而不是 array[0]。另外 Immutable 每次修改都会返回新对象，也很容易忘记赋值。

当使用外部库的时候，一般需要使用原生对象，也很容易忘记转换。

下面给出一些办法来避免类似问题发生：

使用 Flow 或 TypeScript 这类有静态类型检查的工具
约定变量命名规则：如所有 Immutable 类型对象以 $$ 开头。
使用 Immutable.fromJS 而不是 Immutable.Map 或 Immutable.List 来创建对象，这样可以避免 Immutable 和原生对象间的混用。
```





### API

从 JavaScript 数据生成不可变对象 `(支持数据嵌套)`:
将 JavaScript Object 和 Array 彻底转换为 Immutable Map 和 List
```
Immutable.fromJS([1,2])
Immutable.fromJS({a: 1})
```

从 JavaScript 数据生成不可变对象 `(不支持数据嵌套)`:
```
Immutable.List([1,2]})
Immutable.Map({a: 1})
```

从不可变数据生成 JavaScript 对象
```
immutableData.toJS()
```
 
判断两个数据结构是否相等:
```
Immutable.is immutableA, immutableB
```

判断两个数据引用是否一致(shallow equal):
```
immutableA is immutableB
```

### Record
创建一个新的类实例生产记录。记录是类似于一个JS对象,但允许执行一组特定的字符串键,默认值。

```
var ABRecord = Record({a:1, b:2})
var myRecord = new ABRecord({b:3})
```

`Records` 总是为他们定义的键值。切除一个关键的记录只是重置默认值的关键。
```
myRecord.size // 2
myRecord.get('a') // 1
myRecord.get('b') // 3
myRecordWithoutB = myRecord.remove('b')
myRecordWithoutB.get('b') // 2
myRecordWithoutB.size // 2
```

值提供给构造函数中没有记录类型将被忽略,例如,在这种情况下,ABRecord提供一个关键的“x”虽然只有“一个”和“b”的定义,“x”的值为这个记录将被忽略。
```
var myRecord = new ABRecord({b:3, x:10})
myRecord.get('x') // undefined
```

因为记录一组已知的字符串键,property get access works as expected, however property sets will throw an Error.
注意:IE8不支持属性访问。支持IE8时只使用get()。
```
myRecord.b // 3
myRecord.b = 5 // throws Error
```

记录类可以扩展,允许自定义方法在你的记录。这不是一个普遍的模式在功能的环境中,但在许多JS程序。
注意:打印稿不支持这种类型的子类化。
```
class ABRecord extends Record({a:1,b:2}) {
  getAB() {
    return this.a + this.b;
  }
}

var myRecord = new ABRecord({b: 3})
myRecord.getAB() // 4
```

创建 `Recode`
```
Record(defaultValues: {[key: string]: any}, name?: string): Record.Class

const initState = Record({
  siteInfo: {},
  siteStats: {},
  nodes: []
})
```


### List
```
Immutable.List() # 空 List
Immutable.List([1,2])
Immutable.fromJS([1,2])
```

查看 List 的大小:
```
immutableA.size
immutableA.count()
```

判断是否是 List:
```
Immutable.List.isList(x)
```

#### React 组件 `propTypes` 判断是否是 List:
```
React.PropTypes.instanceOf(Immutable.List).isRequired
```

获取 List 索引的元素(负数也是能运行的):
```
immutableData.get(0)
immutableData.get(-1) #反向索引
```

通过 `getIn` 访问嵌套数组当中的数据:
```
immutableData.getIn [1, 2]
```

List 更新操作, 也就是创建一个新的 List 数据:
```
immutableA = Immutable.fromJS([0, 0, [1, 2]])
immutableB = immutableA.set 1, 1
immutableC = immutableB.update 1, (x) -> x + 1
immutableC = immutableB.updateIn [2, 1], (x) -> x + 1
```

排序, 有 `sort` 和 `sortBy`:
```
immutableData.sort (a, b) ->
 if a < b then return -1
 if a > b then return 1
 return 0
immutableData.sortBy (x) ->
  x
```

遍历(返回 `false` 会终止遍历):
```
immutableData.forEach (a, b) ->
  console.log a, b
  return true
```

查找, `find` 返回第一个匹配值, `filter` 返回 List:
```
immutableData.find (x) ->
  x > 1
immutableData.filter (x) ->
  x > 1
immutableData.filterNot (x) ->
  x <= 1
```

### Map
```
Immutable.Map() # 空 Map
Immutable.Map({a: 1})
Immutable.fromJS({a: 1})
```

判断 Map 的写法和 List 判断类似:
```
Immutable.Map.isMap(x)
```

获得 Map 中的数据:
```
immutableData.get('a')
```

通过 `getIn` 访问嵌套的 Map 中的树上:
```
immutableData.getIn ['a', 'b']
```

更新对象和嵌套的对象:
```
immutableB = immutableA.set 'a', 1
immutableB = immutableA.setIn ['a', 'b'], 1
immutableB = immutableA.update 'a', (x) -> x + 1
immutableB = immutableA.updateIn ['a', 'b'], (x) -> x + 1
```

合并对象:
```
immutableB = immutableA.merge(immutableC)
```

判断属性是否存在(`undefined` 也是存在的):
```
immutableData = Immutable.fromJS({key: null})
immutableData.has('key')
```

Map 的 `filter` 和 List 神似, 返回值 Map:
```
data = Immutable.fromJS({a: 1, b: 2})
data.filter (value, key) ->
 value is 1
# => Map {a: 1}
```

同样 Map 也可以做一些 `reduce` 操作, 以及其他的方法, 类似 List:
```
immutableA.reduce (acc, value, key) ->
   acc.set key, value
, immutable.Map()
```

获取 key 和 value 的数组形式:
```
immutableData.keySeq()
immutableData.valueSeq()
```
