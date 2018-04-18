## java.lang.NoSuchFieldError: VERSION_2_3_0

	 java.lang.NoSuchFieldError: VERSION_2_3_0
	        at org.apache.struts2.views.freemarker.FreemarkerManager.getFreemarkerVersion(FreemarkerManager.java:342)
	        at org.apache.struts2.views.freemarker.FreemarkerManager.createConfiguration(FreemarkerManager.java:313)
	        at org.apache.struts2.views.freemarker.FreemarkerManager.init(FreemarkerManager.java:260)
	        at org.apache.struts2.views.freemarker.FreemarkerManager.getConfiguration(FreemarkerManager.java:249)
	        at org.apache.struts2.dispatcher.DefaultDispatcherErrorHandler.init(DefaultDispatcherErrorHandler.java:66)
	        at org.apache.struts2.dispatcher.Dispatcher.init(Dispatcher.java:505)
	        at org.apache.struts2.dispatcher.InitOperations.initDispatcher(InitOperations.java:73)
	        at org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter.init(StrutsPrepareAndExecuteFilter.java:61)
	        at org.apache.catalina.core.ApplicationFilterConfig.initFilter(ApplicationFilterConfig.java:285)
	        at org.apache.catalina.core.ApplicationFilterConfig.getFilter(ApplicationFilterConfig.java:266)
	        at org.apache.catalina.core.ApplicationFilterConfig.<init>(ApplicationFilterConfig.java:108)
	        at org.apache.catalina.core.StandardContext.filterStart(StandardContext.java:4621)
	        at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5266)
	        at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	        at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:754)
	        at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:730)
	        at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:734)
	        at org.apache.catalina.startup.HostConfig.deployWAR(HostConfig.java:985)
	        at org.apache.catalina.startup.HostConfig$DeployWar.run(HostConfig.java:1856)
	        at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	        at java.lang.Thread.run(Thread.java:748)


### bug出现场景
该bug为将 `struts2` 从2.3.16 升级到 2.5.16 后，windows 下部署带 tomcat8.5 没问题，但是 unbuntu 下启动项目时报了该异常

### 出现原因
网上查资料后，得出结论，将 `struts2` 升级后， `freemarker` 版本也需要随之升级；但是记得之前升级的时候，我已经将 `freemarker`升级到最新版，查看 lib 文件夹下的 jar 包，发现 同时存在， freemarker-2.4.jar 和 freemarker-2.3.26-incubating.jar 查看，两个包的结构发现是一样的，所以这里是有冲突的，从而引发了这个bug。<br/>
<br/>
但是我pom明明只配了一个依赖，咋会有两个呢，于是我直接去 `freemarker` [官网](http://mvnrepository.com/artifact/org.freemarker/freemarker) 找到该jar的maven依赖，替换原来自己从阿里云搜索的依赖，问题解决。
<br/><br/>
##### tips: 发现从官网找的maven依赖，和我自己阿里搜的虽然和官网找的包结构是一样的，但是他们的 grounpID 不一致，使用官网的是一定没问题的。 
