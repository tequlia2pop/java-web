# Servlet API

## HTTPServlet

![](images/HttpServlet-API.png)

**javax.servlet.Servlet** 定义了 Servlet 应当有的基本行为。

该接口定义了初始化 servlet、服务请求和从服务器中删除 servlet 的方法。这些方法被称为生命周期方法，并按以下顺序调用：

* 构建 servlet，然后使用 `init` 方法进行初始化。
* 处理所有从客户端到 `service` 方法的调用。
* 取消 servlet 服务，然后使用 `destroy` 方法销毁 servlet，然后执行垃圾收集和 finalized。

**javax.servlet.GenericServlet** 定义一个通用的、与协议无关的 servlet。要编写用于 Web 的 HTTP servlet，需要扩展 `javax.servlet.http.HttpServlet`。

该类的 `service` 方法标记为 abstract。

**javax.servlet.http.HttpServlet** 要创建适合于 Web 站点的 HTTP servlet，应该继承该抽象类。该类的 `service()` 方法通过判断 HTTP 请求的方式（GET、HEAD、POST、PUT、DELETE、OPTIONS、TRACE 等），再分别调用对应的 `doXXX()`。所以如果想针对 GET、POST 等请求进行处理，需要覆盖对应的 `doGet()`、`doPost()` 方法。

注意：这里使用了 Template Method 模式。所以不建议也不应该覆盖 `service()` 方法，这会覆盖掉 `HttpServlet` 中定义的预设处理流程。

## HttpServletRequest & HttpServletResponse

**HttpServletRequest** 表示 HTTP 请求。可以通过它获取 HTTP 请求的相关信息，包括请求参数、标头、上传文件等，还可以进一步设置包含（include）或转发（forward）。在进行请求包含或转发时，若有请求周期内必须共享的资源，则可以设置为请求范围属性。

* getParameter() 根据请求参数的名称获得对应的值。在获取请求参数时，要注意请求对象处理字符编码的问题，才可以正确处理非 ASCII 编码范围的字符。
* getParameterNames() 返回包括所有请求参数名称的 Enumeration。
* getParameterValues()  如果表单上有可复选的元件，如复选框（checkbox）、列表（List）等，则同一个请求参数名称会有多个值（查询字符串类似 `param=10&param=20&param=30`）。这时就可以使用 `getParameterValues()` 方法返回它们的字符串数组。 
* getParameterMap() 返回的 Map 对象的 key 是请求参数名称，value 部分是请求参数值（String[] 的形式，考虑到同一个请求参数有多个值的情况）。

* getHeader() 根据标头的名称获得对应的值。
* getHeaderNames() 返回包括所有标头名称的 Enumeration。
* getHeaders() 

* getPart() 取得 `multipartform-data` 请求的 `javax.servlet.http.Part` 组件。用于进行文件上传处理。

* getRequestDispatcher() 获取 `RequestDispatcher` 对象。`RequestDispatcher` 可以用于将请求转发（forward）到指定的资源，或者将指定的资源包含（include）到响应中。

* getAttribute() 取得指定名称的属性值。
* setAttribute() 设置指定名称的属性值。
* getAttributeNames() 取得所有属性的名称。
* removeAttribute() 移除指定名称的属性。

**HttpServletResponse** 表示 HTTP 响应。可以通过它设置响应类型、标头、缓冲区等，还可以进一步设置重定向、或发送错误状态信息，或者使用 `getWriter().println()` 或 `getOutputStream()` 来输出内容。

* setContentType() 设置响应类型。

* setHeader() 设置响应标头。
* setIntHeader() 设置整数类型的响应标头。
* setDateHeader() 设置日期类型的响应标头。
* addHeader() 附加响应标头。
* addIntHeader() 附加整数类型的响应标头。
* addDateHeader() 附加日期类型的响应标头。

* getBufferSize()
* setBufferSize()
* isCommited() 查看响应是否已提交。
* reset() 重置所有响应信息（包括标头）。
* resetBuffer() 重置响应内容（不包括标头）
* flushBuffer() 清除缓冲区中已设置的响应信息。

* sendRedirect() 重定向。
* sendError() 传送错误状态信息。

## Servelt 的生命周期

请求和响应对象的创建和销毁。

