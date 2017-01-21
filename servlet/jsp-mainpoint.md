# JSP 要点

## JSP 配置

在 web.xml 的 `jsp-config` 元素中可以对所有的 JSP 进行统一的配置：

```xml
<jsp-config>
	<jsp-property-group>
		<url-pattern>*.jsp</url-pattern>
	  	<page-encoding>UTF-8</page-encoding>
	  	<default-content-type>text/html</default-content-type>
		<buffer>16kb</buffer>
		<trim-directive-whitespaces>true</trim-directive-whitespaces>
	</jsp-property-group>
</jsp-config>
```

* page-encoding 指定字符编码。
* default-content-type 指定内容类型
* buffer 指定缓冲区大小。
* trim-directive-whitespaces 是否移除 JSP 指令产生的换行符。

### 关闭 JSP 中的脚本元素

建议使用 EL 来访问服务器端的对象，而不是在 JSP 中编写 Java 代码。因此，可以在 web.xml 中显式地关闭脚本元素：

```xml
<jsp-config>
	<jsp-property-group>
		<url-patterns>*.jsp</url-patterns>
		<scripting-invalid>true</scripting-invalid>
	</jsp-property-group>
</jsp-config>
```

### 关闭 EL 运算

在某些情况下，当你需要在一个 JSP 2.0 或者更新版本的容器上部署 JSP 1.2 应用程序时，就需要关闭 JSP 的 EL 运算。

可以使用 page 指令的 `isELIgnored` 属性：

```
<%@ page isELIgnored="true" %>
```

或者，可以在 web.xml 中显式地关闭 EL 运算：

```xml
<jsp-config>
	<jsp-property-group>
		<url-patterns>/noEl.jsp</url-patterns>
		<el-ignored>true</el-ignored>
	</jsp-property-group>
</jsp-config>
```

## JSP 错误处理

JSP 终究会转译为 Servlet，所以错误可能发生在三个时间：

* JSP 转译为 Servlet 时。如果在 JSP 中编写了错误的语法，使得容器在不知道如何将 JSP 转译为 Servlet，就会发生错误。容器通常无法提供无法转译的原因。确定是否为这类错误的一个原则，就是查看是否有告知语法不合法的信息。
* 编译 Servlet 时。这时会提示 Unable to compile 之类的信息。
* Servlet 载入容器进行服务但发生运行时错误。例如使用 Tomcat，会提示 An exception occurred processing JSP page 之类的信息。

JSP 对错误处理具有很好的支持。可以利用 try 语句处理 Java 代码；也可以指定一个页面，让它在应用程序遇到未捕获的异常时显示出来。

使用 page 指令的 `isErrorPage` 属性可以把一个 JSP 变成一个错误处理页面。要让某个 JSP 在遇到未捕获异常时转到错误处理页面，只需使用 page 指令的 `errorPage` 属性指向错误处理页面即可。

如果想要在容器收到某个类型的异常对象时进行转发，可以在 web.xml 中进行配置：

```
<error-page>
	<exception-type>java.lang.NullPointerException</exception-type>
	<location>/report.view</location>
</error-page>
```

如果想要基于 HTTP 错误状态码转发到处理页面，可以在 web.xml 中进行配置：

```
<error-page>
	<error-code>404</error-code>
	<location>/404.jsp</location>
</error-page>
```






