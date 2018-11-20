`<a>` 标签中使用到Angularjs变量时，点击无效,如果你做了如下配置：

	app.config(['$locationProvider', function($locationProvider) {  
         // $locationProvider.html5Mode(true);  
         $locationProvider.html5Mode({
          enabled: true,
          requireBase: false
        });
        }]);

只需要在 `<a>` 标签中加入 `target="_self"` 属性


其他情况可通过浏览器开发者工具查看是否有事件阻止的情况： [https://www.jianshu.com/p/012aae75b355](https://www.jianshu.com/p/012aae75b355)