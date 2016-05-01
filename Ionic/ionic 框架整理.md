### 目录结构

> css               // 存放css文件＜/br＞
> images            // 存放图片文件＜/br＞
> js                // 存放js文件＜/br＞
>> controllers      // 存放控制器文件＜/br＞
>> directivers      // 存放指令文件＜/br＞
>> lib              // 存放外部包：ionic、require等＜/br＞
>> services         // 存放服务文件＜/br＞
> pages             // 存放页面文件＜/br＞
> app.js            // angularJs启动文件＜/br＞
> bower.json        // Js管理＜/br＞
> index.html        // html入口＜/br＞
> router.js         // 记录路由相关文件＜/br＞


css：
ionic提供了一套友好的APP UI库，用于快速搭建H5页面。＜/br＞
ionic是一套开源的框架，考虑的情况过多，熟悉框架后，可以基于该框架搭建一套自己的angular指令库。＜/br＞
ionic中文网：[http://www.ionic.wang/css_doc-index.html#header](http://www.ionic.wang/css_doc-index.html#header)＜/br＞

### 导航栏/Heather <ion-header-bar>
```
<div class="bar bar-header bar-light">
    <h1 class="title">bar-light</h1>
</div>
```
`<h1>` 标签是导航的标题，ionic的默认设置，ios中该标签是居中显示，而在android中是靠左显示，这里需要在 `app.js` 中加入：
```
$ionicConfigProvider.navBar.alignTitle('center');s
```
这款代码能让顶部导航的标题固定居中显示。

```
<ion-header-bar align-title="center" class="bar-deej">

    <div class="buttons" ng-click="back()">
        <button class="button button-icon icon ion-ios-arrow-back"></button>
    </div>
    
    <h1 class="title">签到</h1>
    
    <div class="buttons" ng-click="submit()">
        <button class="button">提交</button>
    </div>
    
</ion-header-bar>
```
在导航的 `<h1>` 标签的上部、下部加入button后，可以看到页面的顶部导航上的左右各加入了一个按钮，效果就像原生的返回按钮与提交按钮，感觉很酷炫对不对。

### 底部/Footer <ion-footer-bar>
```
<div class="bar bar-footer bar-balanced" ng-click="">
    <div class="title">Footer</div>
</div>
```
底部导航与底部标题，在 `<div>` 上加入 `ng-click` 把他当做底部按钮来玩，目前我就是这么做的，效果还不错噢~

还可以这么玩：
```
<ion-footer-bar align-title="center" class="bar-light">
    <h1 class="title">
            <div class="item-input-inset deej-bottom-input">
            <label class="item-input-wrapper">
                <input type="search" placeholder="说点什么吧..." ng-model="my.comment">
            </label>
            <button class="button button-clear text-light-green size-14" ng-click="comment()">
                发送
            </button>
        </div>
    </h1>
</ion-footer-bar>
```
```
/* 底部输入框 */
.deej-bottom-input {
    padding: 0;
    margin-top: 6px;
}
.deej-bottom-input > label,
.deej-bottom-input > label > input {
    border-radius: 5px;   
}
```
底部放一个输入框，就那他变成一个提交评论的功能了，但是在APP中，点击输入后input框才会显示在输入键盘上，这个应该是ionic的效果吧。
目前只是简单实现了一个提交评论的功能，后续要需要对这块做优化，形成一套指令来玩。

同样的：
```
<ion-footer-bar align-title="left" class="bar-assertive">
    <div class="buttons">
        <button class="button">左侧按钮</button>
    </div>
    <h1 class="title">Title!</h1>
    <div class="buttons" ng-click="doSomething()">
        <button class="button">右侧按钮</button>
    </div>
</ion-footer-bar>
```
他也是可以加入左边的按钮和右边的按钮效果。

### 内容部分/Content <ion-content>
```
<ion-content
    [delegate-handle=""]
    [padding=""]
    [scroll=""]
    [overflow-scroll=""]
    [has-bouncing=""]
    [on-scroll=""]
    [on-scroll-complete=""]>
...
</ion-content>
```
这个东西可吊了，content自带滚动效果，玩了一下他是使用`transform: translate3d(0px, 0px, 0px) scale(1);`来做的，猜测是在js中做了一个动画效果。
在APP中玩就和原生的下拉效果一样样的。
 
标签中的属性在API中均有介绍，目前我并没有一个一个玩过去，日后在玩的时候有什么有趣的在回来记录...
 
#### 下拉刷新/Refresher <ion-refresher>
```
<ion-content ng-controller="MyController">
    <ion-refresher
        pulling-text="下拉刷新..."
        on-refresh="doRefresh()">
    </ion-refresher>
    <ion-list>
        <ion-item ng-repeat="item in items"></ion-item>
    </ion-list>
</ion-content>
``` 
```
angular.module('testApp', ['ionic'])
.controller('MyController', function($scope, $http) {
    $scope.items = [1,2,3];
    $scope.doRefresh = function() {
        $http.get('/new-items')
            .success(function(newItems) {
            $scope.items = newItems;
        })
        .finally(function() {
            // 停止广播ion-refresher
            $scope.$broadcast('scroll.refreshComplete');
        });
    };
});
```
ionic考虑的很全面，在下拉时会出现一段文字，通过 `pulling-text` 触发的，当用户下拉时显示的文字。
在下拉时会出现一个箭头，到达一定的程度箭头会翻转，这个效果ionic也做的很好，通过 `pulling-icon` 当用户下拉时显示的图标。默认: 'ion-arrow-down-c'。
拉到一定距离后IOS、Android会出现一个加载动画，默认是使用与原始相同的动画，还可以自定义动画效果，通过 `on-refresh` 当用户下拉到一定程度然后开始刷新时触发。
ionic有提供一个 `refreshing-icon` `refreshing-text`刷新后显示的图标与文字，但是并没有加入默认配置中，如果有需要可以加入。

#### 容器/Pane <ion-pane>
```
<ion-pane>
  ...
</ion-pane>
```
官方是这么解释的：一个简单的适应内容的容器，无不良影响。在一个元素上添加 'pane'。
还没有使用过不清楚到底是怎么回事咯。

### 滚动容器/Scroll <ion-scroll>
```
<ion-scroll
    [delegate-handle=""]
    [direction=""]
    [paging=""]
    [on-refresh=""]
    [on-scroll=""]
    [scrollbar-x=""]
    [scrollbar-y=""]
    [zooming=""]
    [min-zoom=""]
    [max-zoom=""]>
  ...
</ion-scroll>
```
官方解释是：一个包含所有内容的可滚动容器。
但是我这么使用时：
```
<ion-content>
    <ion-scroll>
    ...
    </ion-scroll>
</ion-content
```
滚动效果是有了，但是他只能在一屏上下滚动，坑爹阿·~~
应该是我使用错了，因为时间的原因没有做过多的尝试。
 
### 底部加载/Loading <ion-infinite-scroll>
```
 <ion-content ng-controller="MyController">
    <ion-infinite-scroll
        ng-if="moreDataCanBeLoaded()"
        on-infinite="loadMore()"
        distance="1%">
    </ion-infinite-scroll>
</ion-content>
```
```
function MyController($scope, $http) {
    $scope.items = [];
    $scope.loadMore = function() {
        $http.get('/more-items').success(function(items) {
        useItems(items);
            $scope.$broadcast('scroll.infiniteScrollComplete');
        });
    };

    $scope.$on('stateChangeSuccess', function() {
        $scope.loadMore();
    });
}
```
这个也很坑，进入页面时就触发事件了，拉倒底部就循环触发事件，说是通过 `ng-if="moreDataCanBeLoaded()` 来结束。
可是宝宝想说，宝宝做了，但是不起效果阿，他一加载就不显示了，直接忽略了这个标签了。
API上也没有做过多的解释，todo 这个一定要搞定他！！！

### $ionicScrollDelegate
这个服务棒棒的，他用于控制 `<ion-content>` 的滚动条。
```
<body ng-controller="MainCtrl">
    <ion-content>
        <button ng-click="scrollTop()">滚动到顶部!</button>
    </ion-content>
</body>

function MainCtrl($scope, $ionicScrollDelegate) {
    $scope.scrollTop = function() {
        $ionicScrollDelegate.scrollTop();
    };
}
```
回到顶部，在标签页切换的时候，content的导航是定在上个页面位置，这个时候就需要用到这个服务，将导航指定到顶部，给用户一个友好的体验嘛。

同理：在发送评论的时候，如果最新评论在最下面的话，为了让用户可以看到自己发送的评论，这个时候指定滚动条到底部的功能 `scrollBottom([shouldAnimate])` 就起作用了

那么我的评论是在顶部的，而且上面还有文章的内容，并且文章内容是不确定高度的，我需要定位到我发送的评论，你怎么办？
我只能说TMD我也想知道怎么办阿，`scrollTo(left, top, [shouldAnimate])` 可以指定滚动条到多少高度，但是如果要实现上面的效果，需要获取到文章内容的高度，然后用做定位的参数，这边具体实现我就不清楚了,囧囧囧囧

但是滚动服务还提供的其他很多方法，如获取 `对象` 滚动到该视图的位置，获取到了要滚到他的位置还不容易吗？
还没有尝试过，其他的方法在后续使用过后加入到文档中。

### 选项卡/Tabs <ion-tabs>
```
<ion-tabs class="tabs-positive tabs-icon-only">

    <ion-tab title="首页" icon-on="ion-ios7-filing" icon-off="ion-ios7-filing-outline">
        <!-- 标签 1 内容 -->
    </ion-tab>

    <ion-tab title="关于" icon-on="ion-ios7-clock" icon-off="ion-ios7-clock-outline">
        <!-- 标签 2 内容 -->
    </ion-tab>

    <ion-tab title="设置" icon-on="ion-ios7-gear" icon-off="ion-ios7-gear-outline">
        <!-- 标签 3 内容 -->
    </ion-tab>

</ion-tabs>
``` 


更新日期：2016-04-30