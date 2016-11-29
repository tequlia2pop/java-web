
WebJars 是打包为 JAR（Java Archive） 文件的客户端 Web 库（例如 jQuery 和 Bootstrap）。

* 在基于 JVM 的 Web 应用程序中显式地和容易地管理客户端依赖
* 使用基于 JVM 的构建工具（例如 Maven，Gradle，sbt，...）下载客户端依赖
* 了解你正在使用的客户端依赖
* 传递依赖性会自动解析并可选择通过 RequireJS 加载
* 部署在 [Maven Central](http://search.maven.org/) 上
* 公共 CDN，由 [JSDELIVR](http://www.jsdelivr.com/) 慷慨提供

WebJars 有三种风格：

* NPM WebJars

	* Instant creation & deployment
	* On-demand releases by anyone
	* Artifact contents mirror NPM package

* Bower WebJars

	* Instant creation & deployment
	* On-demand releases by anyone
	* Artifact contents mirror Bower package

* Classic Webjars
	* 手动打包和部署
	* 由 WebJars 团队按需提供
	* 人工创建 RequireJS 配置