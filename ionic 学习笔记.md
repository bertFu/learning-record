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