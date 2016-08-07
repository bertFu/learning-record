# 目录结构

```
css                     // 存放css文件  
images                  // 存放图片文件  
js                      // 存放js文件  
    - controllers       // 存放控制器文件  
    - directivers       // 存放指令文件  
    - lib               // 存放外部包  
    - services          // 存放服务文件  
pages                   // 存放页面文件  
app.js                  // angularJs启动文件  
bower.json              // Js管理  
index.html              // html入口  
router.js               // 记录路由相关文件
```

## index.html

项目入口，angularJs是一个单页应用，`index.html` 是对外暴露的唯一入口，通过js的路由机制切换页面。

```
<html ng-app="starter">
```

通过 `ng-app` 来指定angularJs的主模块。

## app.js

模块主入口，整合所有js，初始化项目。

```
angular.module('starter', ['ionic', 'starter.services', 'starter.consultant'])
```

创建模块，angularJs中有一句话，一切从模块开始。

```
angular.module('模块名', ['关联模块'])
```

模块名：在 `index.html` 中的 `ng-app` 引入模块名来指定模块。

通过 `run()`、`config()`、`constant()` 来初始化模块。

## routet.js

angularJs的路由机制，用于页面跳转，将视图与控制器做关联。 angularJs自带的 `angular-router` 对于二级页面切换的功能不够强大，所以我们选择使用 `angular-ui-router`

```
angular.module('starter')
    .config(function($stateProvider, $urlRouterProvider) {

        $stateProvider
            .state('service', {
                url: '/service?memberId',
                templateUrl: 'pages/service.html',
                controller: 'ServiceCtrl'
            })

            $urlRouterProvider.otherwise('/error');
    });
```

`state('路由名称')`： 定义路由名称。<br>
`url`： 指定路由的路径，angular的单页应用是基于html的锚标记来做处理的，所以在页面中需要使用 `#/service` 来跳转，`#` 表示在单页跳转，js判断后面的路径来做相应的处理。 `templateUrl`： 指定相应的视图，js判断出对应的路由地址，会将 `templateUrl` 中对应的视图模块渲染到页面上。 `controller`：指定视图对应的控制器，简单的来说就是 `router.js` 帮助视图与控制器做整合，每个视图有自己的js文件，控制器中对服务端传来的数据做处理后渲染到对应的视图中。 `$urlRouterProvider.otherwise('/error');`：当 `router,js` 没有找到相应的路由路径时，会跳转到默认路径，由 `$$urlRouterProvider.otherwise('/error');` 来指定默认路由。

## template

视图模板，模块只关心呈现到页面的效果，通过css和html来搭建出页面效果，获取数据并渲染。

## controller.js

`controller` 的职责是对数据处理。

## service.js

`service` 提供服务，目前我将他当做dao来使用，对于前端工程来说，服务端就类似于数据库，通过 `ajax` 请求需要的数据，返回给调用的控制器。

现在有一个混乱点，就是 `controller` 中的数据共享也是通过 `service` 来做的，这里他的职责变成了一个类似类的东西，里面有属性，方法。<br>
他存储了需要的数据，在控制器中传递。

有的时候一个控制器里会传入很多的服务对象，在这里还是挺纠结的，这样做真的好吗？？？
