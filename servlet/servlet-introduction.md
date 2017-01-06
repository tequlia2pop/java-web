# Servlet 介绍

## MVC/Model 2

MVC（Model、View、Controller，模型、视图、控制器）

**应用于桌面应用程序的 MVC 模式**

![](images/mvc-desktop.png)

**应用于 Web 应用程序的 MVC 模式**

![](images/mvc-web.png)

桌面应用程序和 Web 应用程序的 MVC 模式有一个决定性的不同：由于 Web 应用程序是基于 HTTP 的请求/响应模型，没有请求就不会有响应，也就是 HTTP 服务器不可能主动对浏览器发出响应。

在 Model 2 架构下，控制器、模型、视图的职责如下：

* 控制器：取得和验证请求参数，委托模型处理，转发请求给视图。不应该出现 HTML 代码，不涉及业务逻辑。

* 模型：接受控制器的请求调用，负责处理业务逻辑和数据存取逻辑等。根据应用程序功能，产生多种不同职责的模型对象。不应该出现 HTML 代码。

* 视图：接受控制器的请求调用，从模型提取运算结果（模型通常将结果设置为各种作用域对象的属性），根据需求呈现所需的画面。不应该出现程序代码。

许多 Web 框架都实现了 Model 2。

## Web 目录结构

![](images/Web应用程序文件结构.png)

Web 目录结构必须符合一定的规范：

* **WEB-INF** 客户端无法直接访问 WEB-INF 中的内容。如果要访问其中的内容，必须通过 Servlet/JSP 的请求转发（Forward）。

* **WEB-INF/web.xml** Web 应用程序的部署描述文件。

* **WEB-INF/classes** 放置 .class 文件的目录。

* **WEB-INF/lib** 放置 JAR 文件的目录。

	* JAR 中可以放置 Servlet/JSP、自定义类、工具类、部署描述文件等，应用程序的的类加载器可以从 JAR 中载入对应的资源。
	* 如果要使用某个类，Web 应用程序会首先去 `WEB-INF/classes` 中试着加载类；若无，再试着从 `WEB-INF/lib` 的 JAR 文件中寻找类文件；如果还没有找到， 则会到容器实现本身存放类或 JAR 的目录中寻找。
	* 可以在 JAR 文件的 `/META-INF/resources` 目录中放置静态资源或 JSP 等。例如如果在 `/META-INF/resources` 中放入 `index.html`，若请求的 URL 中包括 `/openhome/index.html`，但实际上 `/openhome` 根目录下不存在 `index.html`，则会使用 JAR 文件中的 `/META-INF/resources/index.html`。 

如果 Web 应用程序的 URL 最后是以 "/" 结尾，

* 如果不存在该目录，则会使用预设的 Servlet。

* 如果存在该目录，则 Web 容器必须传回该目录下的欢迎页面。可以在 web.xml 中包括以下的定义，指出可用的欢迎页面。

	```xml
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
	```

	如果找不到以上的文件，则会尝试从 JAR 文件的 `META-INF/resources` 中寻找对应的资源页面。

## 使用 web-fragment.xml

P54

## Web 容器

Web 容器负责持有 Servlet 实例，还负责 Servlet 的生命周期和相关服务的连接。

* 在具体层面上，Web 容器就是一个 Java 程序。
* 在抽象层面上，可以将 Web 容器视为运行 Servlet/JSP 的 HTTP 服务器。

请求/响应的基本流程：

1. 客户端（大部分情况下是浏览器）对 Web 服务器发出 HTTP 请求。
1. HTTP 服务器接收到 HTTP 请求，将请求转由 Web 容器处理。Web 容器会剖析 HTTP 请求内容，创建各种对象，如代表请求的 `HttpServletRequest` 对象、代表响应的 `HttpServletResponse` 对象、`HttpSession` 对象等。
1. Web 容器根据 `@WebServlet` 注解或 web.xml 设置，决定使用哪个 Servlet 来处理请求，然后调用它的 `service()` 方法，`service()` 方法根据 HTTP 请求的方式掉哟哦那个对应的 `doXXX()` 方法。
1. 在 `doXXX()` 方法中，使用请求对象（`HttpServletRequest`）来获得请求相关的信息，使用响应对象（`HttpServletResponse`）来创建响应。
1. Web 容器将响应转换为 HTTP 响应，再由 HTTP 服务器对客户端进行响应。
1. 最后，Web 容器将 `HttpServletRequest`、`HttpServletResponse` 对象销毁回收，该次请求响应结束。

![](images/请求和响应的基本示例.png)

Web 服务器会为每个请求分配一个线程。注意，Web 容器可能会使用同一个 Servlet 实例来服务多个请求。也就是说，在多个请求下，就相当于多个线程在共同存取一个对象，因此得注意线程安全问题。

![](images/请求响应的时序图.png)

## Servlet

Servlet 必须继承 `HttpServlet`。

不应该直接在 Servlet 中直接产生视图内容（如直接编写 HTML）。

`HttpServletRequest` 表示 HTTP 请求。可以通过它获取 HTTP 请求的相关信息，包括请求参数、标头、上传文件等，还可以进一步设置包含（include）或转发（forward）。在进行请求包含或转发时，若有请求周期内必须共享的资源，则可以设置为请求范围属性。

注意：在取得请求参数的时候，要注意请求对象处理字符编码的问题。

`HttpServletResponse` 表示 HTTP 响应。可以通过它设置响应类型、标头、缓冲区等，还可以进一步设置重定向、或发送错误状态信息，或者使用 `getWriter().println()` 或 `getOutputStream()` 来输出内容。

注意：为了让浏览器知道如何处理响应的内容，记得设置正确的内容类型（Content Type）。

## JSP

JSP 实际上就是 Servlet，它最终会由 Web 容器编译为 Servlet 并加载执行。必要时可配合适当的工具，查看 JSP 编译为 的 Servlet 之后的源代码。