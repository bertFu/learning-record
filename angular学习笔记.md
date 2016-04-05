### angular学习笔记

1. 初次使用 `angular-ui-router` 据说是可以多层嵌套路由。

```
bower install angular-ui-router --save
```
使用 `bower` 安装 `angular-ui-router` 并将版本信息保存在 `bower.json`中。

与 `angular-router` 使用方式类似。

```
angular.module('sntApp')
    .config(function($stateProvider, $urlRouterProvider) {
        $urlRouterProvider.otherwise('/login');
        $stateProvider
            .state('index', {
                url: '/',
                views: {
                    '': {
                        templateUrl: 'pages/index.html',
                        controller: 'IndexCtrl',
                        css: 'style/index.css'
                    }   
                }              
            })
            .state('login', {
                url: '/login',
                templateUrl: 'pages/login.html',
                controller: 'LoginCtrl',
                css: 'style/login.css'
            })
    })
```
通过 `stateProvider` 来控制路由。
`when -> state` 设置路由状态。
`urlRouterProvider` 设置默认路由。
`url` 路由路径。
`templateUrl` 视图模板地址。
`controller` 控制器，可以是名称、Function、js文件。
`views` 内部如有多个视图时使用，可以通过 `ui-view` 来嵌入相应视图。


#### 神坑模块之 `angular-css`

`angular-css` 的使用非常之简单可以通过 [CSS on-demand for AngularJS](https://github.com/castillo-io/angular-css#for-states-ui-router)来学习。

不过作者有点坑，在使用中，发现js无法正常执行。

```
angular.js:13294 Error: [orderBy:notarray] Expected array but received: 0
http://errors.angularjs.org/1.5.2/orderBy/notarray?p0=0
    at angular.js:68
    at angular.js:20511
    at fn (eval at <anonymous> (angular.js:14138), <anonymous>:4:176)
    at trackByIdExpFn (angular.js:28596)
    at ngRepeatAction (angular.js:28653)
    at $watchCollectionAction (angular.js:16604)
    at Scope.$digest (angular.js:16739)
    at Scope.$apply (angular.js:17003)
    at done (angular.js:11324)
    at completeRequest (angular.js:11522)
```
出现这样的错误，经过一整晚对源代码的研究发现，错误原来如此美妙。。。。

```
var template = '<link ng-repeat="stylesheet in stylesheets track by $index | orderBy: \'weight\' " rel="{{ stylesheet.rel }}" type="{{ stylesheet.type }}" ng-href="{{ stylesheet.href }}" ng-attr-media="{{ stylesheet.media }}">';
```
作者的这段代码中 `track by $index` 与 `| orderBy: \'weight\'` 发生冲突，解决方案，删除其中一个。
当我发现这个错误时，我一直以为作者是对的，应该是我某个方面操作不恰当导致程序出错，百度，Google找了老半天也没有找到相关文章。
带着疑惑，我把这段代码贴到 `GiHubt` 上，在Issues中找到了同道中人。
[Error: [orderBy:notarray] Expected array but received: 0 ](https://github.com/castillo-io/angular-css/issues/60)
原来我的问题老外们也遇到过，解决方法也是我想的这样。
虽然这一晚上只处理了作者的一个小bug，但是收获还是挺丰富的，相信在使用这套js的时候应该会更得心应手。
更重要的一点是，GitHub真是一个好东西阿。

碎觉碎觉，本以为今晚会是一个不眠之夜，幸灾，幸灾。