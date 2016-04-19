### ion-nav-view
`ion-nav-view` 是对 `ui-view` 做了扩展，在视图切换时会有滑动效果。
`ion-nav-view` 内只用 `div` 标签做容器的情况下，两层跳转就会导致黑屏。
`ion-nav-view` 需要配合 `ion-view` 来使用。

[Ionic Javascript APi |ion-nav-view导航栏视图]( http://ngionic.com/2014/12/ionic-javascript-api-ion-nav-view%E5%AF%BC%E8%88%AA%E6%A0%8F%E8%A7%86%E5%9B%BE/)

1、讲解了 `ion-nav-view` 如何使用。
     -通过 `$stateProvider` 匹配状态，加载到 `<ion-nav-view>` 标签中。
     -通过使用 `angularJs Caching` 缓存技术缓存视图，用于滚动上一个视图。
2、全局缓存禁用
```
$ionicConfigProvider.views.maxCache(0);
```
3、局部缓存禁用
```
局部缓存禁用
```
4、通过属性禁用缓存
```
<ion-view cache-view="false" view-title="My Title!">
  ...
</ion-view>
```

请访问 [AngularUI Router’s docs](https://github.com/angular-ui/ui-router/wiki) 了解更多。


### ion-tabs
- 选项卡：用于却换整组页面。
- 一般用于底部导航或顶部导航。
- 在IOS、Android显示效果不同：IOS默认是底部导航；Android默认是顶部导航。
```
$ionicConfigProvider.tabs.position('bottom'); // 设置 tabs 默认在底部
$ionicConfigProvider.navBar.alignTitle('center'); // 设置 navBar 标题默认居中显示
```

> 用法
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

### $ionicTabsDelegate
- 该方法直接调用$ionicTabsDelegate服务，控制所有ionTabs指令。用$getByHandle方法控制具体的ionTabs实例

> 用法：在其他标签页面需要切换标签页时操作
```
<body ng-controller="MyCtrl">
  <ion-tabs>

    <ion-tab title="Tab 1">
      你好，标签1！
      <button ng-click="selectTabWithIndex(1)">选择标签2</button>
    </ion-tab>
    <ion-tab title="Tab 2">你好标签2！</ion-tab>

  </ion-tabs>
</body>
```
```
function MyCtrl($scope, $ionicTabsDelegate) {
  $scope.selectTabWithIndex = function(index) {
    $ionicTabsDelegate.select(index);
  }
}
```

> 方法：
```
select(index, [shouldChangeHistory]) // 选择标签来匹配给定的索引。
```
 