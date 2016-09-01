# box-shadow

> text-shadow是给文本添加阴影效果，box-shadow是给元素块添加周边阴影效果。

## 基本语法:

```
box-shadow: h-shadow v-shadow blur spread color inset;
```

{box-shadow:[投影方式] X轴偏移量 Y轴偏移量阴影模糊半径 阴影扩展半径 阴影颜色}

box-shadow 向框添加一个或多个阴影。该属性是由逗号分隔的阴影列表，每个阴影由 2-4 个长度值、可选的颜色值以及可选的 inset 关键词来规定。省略长度的值是 0。

值        | 描述                        | 使用情况
:------- | :------------------------ | :------------------------------------------------------------------------------------------------------
h-shadow | 必需。水平阴影的位置。允许负值。          | 如果值为正值，则阴影在对象的右边，其值为负值时，阴影在对象的左边；
v-shadow | 必需。垂直阴影的位置。允许负值。          | 如果为正值，阴影在对象的底部，其值为负值时，阴影在对象的顶部；
blur     | 可选。模糊距离。                  | 如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
spread   | 可选。阴影的尺寸。                 | 如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；
color    | 可选。阴影的颜色。请参阅 CSS 颜色值。     | 如不设定颜色，浏览器会取默认色，但各浏览器默认取色不一致，特别是在webkit内核下的safari和chrome浏览器下表现为透明色，在Firefox/Opera下表现为黑色（已验证），建议不要省略此参数。
inset    | 可选。将外部阴影 (outset) 改为内部阴影。 | 如果设置则阴影在内部，不设置阴影在外部，默认在外部

## 浏览器兼容

IE          | Firefox        | Chrome     | Opera      | Safari  | 是否支持
:---------- | :------------- | ---------- | ---------- | ------- | ----
IE6、IE7、IE8 | Firefox 3.0.10 |            | Opera 9.64 |         | 不支持
IE9         | Firefox 3.5    | Chrome 2.0 | Opera 10.6 | Safari4 | 不支持

为了兼容各主流浏览器并支持这些主流浏览器的较低版本，在基于Webkit的Chrome和Safari等浏览器上使用box-shadow属性时，我们需要将属性的名称写成 `-webkit-box-shadow` 的形式。Firefox浏览器则需要写成 `-moz-box-shadow` 的形式。

```
.box-shadow{  

         //Firefox4.0-  

         -moz-box-shadow:投影方式 X轴偏移量 Y轴偏移量阴影模糊半径 阴影扩展半径 阴影颜色;  

         //Safariand Google chrome10.0-  

         -webkit-box-shadow:投影方式 X轴偏移量 Y轴偏移量阴影模糊半径 阴影扩展半径 阴影颜色;  

         //Firefox4.0+、 Google chrome 10.0+ 、 Oprea10.5+ and IE9  

         box-shadow:  投影方式 X轴偏移量 Y轴偏移量 阴影模糊半径 阴影扩展半径 阴影颜色;  

}
```

## 参考资料

- [CSS3阴影 box-shadow的使用和技巧总结](http://blog.csdn.net/freshlover/article/details/7610269)

  - 里面对 `box-shadow` 的使用例子做了全面描述，在做高级处理时可以参考
