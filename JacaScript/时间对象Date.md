# Date

> Date 类型提供了有关日期和时间的信息，包括当前日期和时间以及相关的计算功能。

创建日期对象：

```javascript
var now = new Date();
```

构造函数不传参数情况下，新对象自动获得当前日期和时间。

如果想要更具特定的日期和时间创建日期对象，必须传入表示该日期的毫秒数（即从UTC时间1970年1月1日午夜起至该日期止经过的毫秒数）。

为了简化这一计算过程，ECMAScript 提供了两个方法：

- Date.parse()：该方法接受一个表示日期的字符串参数，然后尝试根据这个字符串返回相应日期的毫秒数。

  - ECMA-262没有定义 Date.parse() 应该支持那种日期格式，因此这个方法的行为因实现而异，而且通常是因地区而异。
  - "月/日/年"，如 "6/13/2004"。
  - "因为月名 日，年"，如 "January 12,2004"。
  - "因为星期几 因为月名 日 年 时:分:秒 时区"，如 "TueMay 25 2004 00:00:00 GMT-0770"。
  - ISO 8601扩展格式YYYY-MM-DDTHH:mmSS.SSZ (例如："2004-05-25T00:00:00")。只有兼容ECMAScript 5的实现支持这种格式。

    > 在一次项目的扩展中，使用 YYYY-MM-DDTHH:mmSS.SSZ 这个格式在 Safari 上就出现无法解析的问题，解决方式是把 '-' 转 成 '/'。

  ```javascript
  // 创建 2004年5月25日的一个日期对象
  var someDate = new Date(Date.parse("May 25,2004"));

  // 如果传入Date.parse() 方法的字符串不能表示日期，那么它会返回NAN。实际上，如果直接将表示日期的字符创传递给Date构造函数，也会在后台调用 Date.parse()：
  var someDate = new Date("May 25,2004"); // 这句代码与上句代码是等价的
  ```

  > 日期对象在不同浏览器中的实现有许多起卦的行为。其中有一种倾向是将超出范围的值替换成当前值，以便生成输出。

  > 例如：在接续 "January 32,2007"时，有的浏览器会将其解释为 "February 1,2007"。而Opera则倾向于插入当前月份的当前日期，返回 "January当前日期，2007"。

  > 也就是说，如果在2007年9月21日运行前面的代码，将会得到 "January 21,2007"（都是21日）。

- Date.UTC()：

> Date.UTC()方法同样也返回表示日期的毫秒数，但它与 Date.parse() 在构建值时使用不同的信息。

- 参数一：年份、基于0的月份（一月是0，二月是1，以此类推）
- 参数二：月中的哪一天（1到31）
- 参数三：小时数（0到23）
- 参数四：分钟
- 参数五：秒
- 参数六：毫秒数

在这些参数中只有前两个参数（年和月）是必需的。

```javascript
// GMT时间2000年1月1日午夜零时
var y2k = new Date(Date.UTC(2000, 0));

// GMT时间2005年5月5日下午5.:55:55
var allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));

// 本地时间2000年1月1日午夜零时
var y2k_1 = new Date(2000, 0);

// 本地时间2005年5月5日下午5:55:55
var allFives_1 = new Date(2005, 4, 5, 17, 55, 55);
```

ECMAScript 5 添加了 Date.now() 方法，返回表示调用这个方法时的日期和时间的毫秒数。这个方法简化了使用Date对象分析代码的工作。

```javascript
// 取得开始时间
var start = Date.now();

// 调用函数
doSomething()

// 取得停止时间
var stop = Date.now(),
    result = stop - start;
```

获取一个方法的执行时间毫秒值

支持 Date.now() 方法的浏览器包括 IE9+、Firefox 3+、Safari 3+、Opera 10.5和Chrome。

```javascript
// 取得开始时间
var start = +new Date();

// 调用函数
doSomething()

// 取得停止时间
var stop = +new Date();
    result = stop - start;
```

在不支持 Date.now() 方法的浏览器中，使用 '+' 操作符把Data对象转换成字符串，也可以得到毫秒值。

## 比较 Date 的大小

Date 类型的 ValueOf() 方法，不返回字符串，而是返回日期的毫秒表示。因此，可以方便使用比较操作符（大于或小于）来比较日期值。

```javascript
var date1 = new Date(2007, 0, 1);
var date2 = new Date(2007, 1, 1);

alert(date1 < date2) // true
alert(date1 > date2) // false
```

## 日期格式化

Date 类型还有一些专门用于将日期格式化为字符串的方法：

- toDateString()：`new Date().toDateString() // "Tue Oct 18 2016"`
- toTimeString()：`new Date().toTimeString() // "23:29:12 GMT+0800 (CST)"`
- toLocaleDateString()：`new Date().toLocaleDateString() // "2016/10/18"`
- toLocaleTimeString()：`new Date().toLocaleTimeString() // "下午11:30:28"`
- toUTCString()：`new Date().toUTCString() // "Tue, 18 Oct 2016 15:30:53 GMT"`

## 日期/时间组件方法

方法                   | 说明
:------------------- | :--------------------------------------------------------
getTime()            | 返回表示日期的毫秒数：与valueOf()方法返回的值相同
setTime(毫秒)          | 以毫秒数设置日期，会改变整个日期
getFullYear()        | 获取四位数的年丰（如 2007而非07）
getUTCFullYear()     | 返回UTC日期的4位数年份
setFullYear(年)       | 设置日起的年份，传入的年分支必须是4为数字（如 2007而非07)
setUTCFullYear(年)    | 设置UTC日期的年份。传入的年份值必须是4位数字（如 2007而非07)
getMonth()           | 返回日期中的月份，其中0表示一月，11表示十二月
getUTCMonth()        | 返回UTC日期中的月份，其中0表示一月，11表示十二月
setMonth()           | 设置日期的月份。传入的月份值必须大于0，超过11则增加年份
setUTCMonth()        | 设置UTC日期的月份。传入的月份值必须大于0，超过11则增加年份
getDate()            | 返回日期月份中的天数（1到31）
getUTCDate()         | 返回UTC日期月份中的天数（1到31）
setDate()            | 设置日期月份中的天数。如果传入的值超过了该月中应有的天数，则增加月份
setUTCDate(日)        | 设置UTC日期月份中的天数。如果传入的值超过了该月中应有的天数，则增加月份
getDay()             | 返回日期中清气得星期几（其中0表示星期日，6表示星期六）
getUTCDay()          | 返回UTC日期中清气得星期几（其中0表示星期日，6表示星期六）
getHours()           | 返回日期中的小时数（0到23）
getUTCHours()        | 返回UTC日期中的小时数（0到23）
setHours(时)          | 设置日期中的小时数，传入的超过了23则增加月份中的天数
setUTCHours(时)       | 设置UTC日期中的小时数，传入的超过了23则增加月份中的天数
getMinutes()         | 返回日期中的分钟数（0到59）
getUTCMinutes()      | 返回UTC日期中的分钟数（0到59）
settMinutes(分)       | 设置日期中的分钟数。传入的值超过59则增加小时数
settUTCMinutes(分)    | 设置UTC日期中的分钟数。传入的值超过59则增加小时数
getSeconds()         | 返回日期中的秒数（0到59）
getUTCSeconds()      | 返回UTC日期中的秒数（0到59）
setSeconds(秒)        | 设置日期中的秒数，传入的值超过59则增加分钟数
setUTCSeconds(秒)     | 设置UTC日期中的秒数，传入的值超过59则增加分钟数
getMillSeconds()     | 返回日期中的毫秒数
getUTCMillSeconds()  | 返回UTC日期中的毫秒数
settMillSeconds()    | 设置日期中的毫秒数
settUTCMillSeconds() | 设置UTC日期中的毫秒数
getTimezoneOffset()  | 返回本地时间与UTC时间相差的分钟数。例如：美国东部标准时间返回300。在某地进入夏令时的情况下，这个值会有所变化
