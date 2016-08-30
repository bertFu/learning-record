# JS 控制滚动条操作

今天玩 `antd-mobile` 碰到一个滚动条的问题：在 `Tab` 组件中使用多个 `ListView` ，当操作第一个 `Tab` 中的 `ListView A` 组件，加载数据，滚动，底部加载一切正常，可是当我点击切换到第二个 `Tab` 中的 `ListView B` 组件时，发现在 `ListView B` 组件中滚动屏幕，触发了 `ListView A` 组件中的滚动事件，同样的，在 `ListView A` 组件中滚动屏幕，也会触发 `ListView` 组件的滚动事件。

导致这个奇葩问题的原因居然是两个 `ListView` 公用了 `body` 的 `scroll`，解决思路就是让两个 `ListView` 视同自己的 `scroll` 事件，同时设置 `body` 的 `overflow: hidden`。

反思了下对 `css`、`js`、`html` 的滚动相关操作的技能太浅，特别整理了这篇笔记。

## Css 属性 overflow

> 设置内容溢出显示状态

```css
div
  {
  width:150px;
  height:150px;
  overflow:scroll;
  }
```

设置div内容溢出时显示滚动条

可能的值

值       | 描述
:------ | :---------------------------
visible | 默认值。内容不会被修剪，会呈现在元素框之外。
hidden  | 内容会被修剪，并且其余内容是不可见的。
scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。
inherit | 规定应该从父元素继承 overflow 属性的值。

## Js 监听滚动事件

```javascript
window.addEventListener('scroll', this.handleScroll); // 添加滚动事件
window.removeEventListener('scroll', this.handleScroll); // 删除滚动事件

handleScroll: function (e) {
  console.log('浏览器滚动事件')
}
```

## Js 获取与滚轮与顶部距离小技巧

> 各浏览器下 scrollTop的差异 IE6/7/8：

> - 对于没有doctype声明的页面里可以使用 document.body.scrollTop 来获取 scrollTop高度 ；
> - 对于有doctype声明的页面则可以使用 document.documentElement.scrollTop； Safari:
> - safari 比较特别，有自己获取scrollTop的函数 ： window.pageYOffset ； Firefox:
> - 火狐等等相对标准些的浏览器就省心多了，直接用 document.documentElement.scrollTop ；

`chrome` 不认识 `document.documentElement.scrollTop`

如果有文档声明（即网页第一句的docType）的情况下，标准浏览器是只认识 `documentElement.scrollTop` 的，但chrome虽然我感觉比firefox还标准，但却不认识这个，在有文档声明时，chrome也只认识document.body.scrollTop.

由于在不同情况下，document.body.scrollTop与document.documentElement.scrollTop都有可能取不到值

document.body.scrollTop与document.documentElement.scrollTop两者有个特点，就是同时只会有一个值生效。比如document.body.scrollTop能取到值的时候，document.documentElement.scrollTop就会始终为0；反之亦然。所以，如果要得到网页的真正的scrollTop值

```javascript
// 方法一
var scrollTop = document.body.scrollTop + document.documentElement.scrollTop;
var scrollLeft = document.body.scrollLeft + document.documentElement.scrollLeft;

// 方法二
var scrollLeft = Math.max(document.documentElement.scrollLeft, document.body.scrollLeft);
var scrollTop = Math.max(document.documentElement.scrollTop, document.body.scrollTop);

// 方法三
var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop || 0;
```

这样就可以解决距离顶部的问题。

## Js 判断滚动到底部

判断滚动条到底部，需要用到DOM的三个属性值，即scrollTop、clientHeight、scrollHeight。

- `scrollTop` 为滚动条在Y轴上的滚动距离。
- `clientHeight` 为内容可视区域的高度。
- `scrollHeight` 为内容可视区域的高度加上溢出（滚动）的距离。

从这个三个属性的介绍就可以看出来，滚动条到底部的条件即为scrollTop + clientHeight == scrollHeight

```javascript
//滚动条在Y轴上的滚动距离
function getScrollTop(){
　　return  scrollTop = document.body.scrollTop + document.documentElement.scrollTop;
}
//文档的总高度
function getScrollHeight(){
  return scrollHeight = document.body.scrollHeight + document.documentElement.scrollHeight;
}
//浏览器视口的高度
function getWindowHeight(){
　　return document.compatMode == "CSS1Compat" ? windowHeight = document.documentElement.clientHeight : windowHeight = document.body.clientHeight;
}
window.onscroll = function(){
　　if(getScrollTop() + getWindowHeight() == getScrollHeight()){
　　　　alert("you are in the bottom!");
　　}
};
```

## 带动笑的滚动

使用 `Jquery` 实现

```
$('div').click(function(){$('html,body').animate({scrollTop:$('.a').offset().top}, 800);});
```

## Js 滚动相关API

- 网页可见区域宽： document.body.clientWidth;
- 网页可见区域高： document.body.clientHeight;
- 网页可见区域宽： document.body.offsetWidth; (包括边线的宽)
- 网页可见区域高： document.body.offsetHeight; (包括边线的宽)
- 网页正文全文宽： document.body.scrollWidth;
- 网页正文全文高： document.body.scrollHeight;
- 网页被卷去的高： document.body.scrollTop; （当前滚动条距离顶部的距离）
- 网页被卷去的左： document.body.scrollLeft;（当前滚动条距离左边的距离）
- 网页正文部分上： window.screenTop;
- 网页正文部分左： window.screenLeft;
- 屏幕分辨率的高： window.screen.height;
- 屏幕分辨率的宽： window.screen.width;
- 屏幕可用工作区高度： window.screen.availHeight;
- 屏幕可用工作区宽度：window.screen.availWidth;
- scrollHeight: 获取对象的滚动高度。
- scrollLeft:设置或获取位于对象左边界和窗口中目前可见内容的最左端之间的距离
- scrollTop:设置或获取位于对象最顶端和窗口中可见内容的最顶端之间的距离
- scrollWidth:获取对象的滚动宽度
- offsetHeight:获取对象相对于版面或由父坐标 offsetParent 属性指定的父坐标的高度
- offsetLeft:获取对象相对于版面或由 offsetParent 属性指定的父坐标的计算左侧位置
- offsetTop:获取对象相对于版面或由 offsetTop 属性指定的父坐标的计算顶端位置
- event.clientX 相对文档的水平座标
- event.clientY 相对文档的垂直座标
- event.offsetX 相对容器的水平坐标
- event.offsetY 相对容器的垂直坐标
- document.documentElement.scrollTop 垂直方向滚动的值
- event.clientX+document.documentElement.scrollTop 相对文档的水平座标+垂直方向滚动的量
- 要获取当前页面的滚动条纵坐标位置，用：
- document.documentElement.scrollTop;
- 而不是：
- document.body.scrollTop;
- documentElement 对应的是 html 标签，而 body 对应的是 body 标签

## 拓展

- [JS滚轮事件(mousewheel/DOMMouseScroll)了解](http://www.zhangxinxu.com/wordpress/2013/04/js-mousewheel-dommousescroll-event/)

  - 一张较全的IE7, IE10, Chrome, 以及FireFox，鼠标向下滚动图表
  - 讲解了 `DOMMouseScroll`、 `onmousewheel`的差异
  - 兼容的滚轮事件方法
  - 简单的实例
