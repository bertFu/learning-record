# Map

```
var Immutable = require('immutable');

var map1 = Immutable.Map({a:1, b:2, c:3});
var map2 = map1.set('b', 50);

map1.get('b'); // 2
map2.get('b'); // 50
```

使用 `Map` 创建创建的数据，通过 `set()` 更新数据后产生新的的数据 `map2`。<br>
`map1 === map2 // false`<br>
`map1` 与 `map2` 共享数据 `a` 和 `c`，但是 `b` 是变化数据。 对比深拷贝，这样的方式能提升代码性能。
