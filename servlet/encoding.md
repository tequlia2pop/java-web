# 字符编码处理

## URL 编码

**保留字符**

URI 规范定义了一些保留字符（reserved character）。如果要在请求参数上使用这些保留字符，必须使用 URI 规范的百分比编码（Percent-Encoding），也就是俗称的 URI 编码或 URL 编码。具体来说，必须在 '%' 字符之后以十六进制数值表示方式，来表示该字符的八个位数值。

保留字符 | 编码值
------- | -----
: | %3A
/ | %2F
? | %3F
& | %26
= | %3D
@ | %40
% | %25
空格符 | %20

可以使用 Java 的 `java.net.URLEncoder` 和 `java.net.URLDecoder` 来执行 URL 编码转换。

**中文字符**

UTF-8 编码与 ASCII 字符的编码部分是兼容的，也就是使用一个字节。但是中文在 UTF-8 编码下会使用三个字节来表。根据网页的编码方式（例如 UTF-8 编码），浏览器会自动对请求参数中的中文执行 URL 编码；服务器端处理请求参数时应该以对应的编码方式对其进行解码。

## 请求参数的编码处理

**POST 请求参数的编码处理**

如果客户端没有在 `Content-Type` 标头中设置字符编码信息，此时 `HttpServletRequest` 的 `getCharacterEncoding()` 会返回 `null`，那么容器就会使用默认的字符编码来解析请求参数（大部分浏览器默认的字符集是 ISO-8859-1）。

可以显式指定取得 POST 请求参数时要使用哪种字符编码：

```java
// POST 的编码处理
request.setCharacterEncoding("UTF-8");
```

**GET 请求参数的编码处理**

`HttpServletRequest#setCharacterEncoding()` 方法只对请求 body 中的内容有效，也就是说基本上该方法只对 POST 产生作用。

可以通过 `String#getBytes()` 指定编码取得该字符串的字节数组，然后再重新构造为正确编码的字符串：

```java
String name = request.getParameter("nameGet");
// GET 的编码处理
name = new String(name.getBytes("ISO-8859-1"), "UTF-8");
```

## HttServletResponse 的编码处理

`HttServletResponse` 使用的字符编码默认是 ISO-8859-1。所以，如果要使用 `HttServletResponse` 输出字符，可以显式为它设置要使用的字符编码。

**设置 Locale**

地区（Locale）信息包括了语系和编码信息。语系信息通常通过响应标头 Content-Language 来设置。

```java
response.setLocale(Locale.TAIWAN);
```

可以在 web.xml 中设置默认的区域与编码对应：

```xml
<locale-encoding-mapping-list>
	<locale-encoding-mapping>
		<locale>zh_TW</locale>
		<encoding>UTF-8</encoding>
	</locale-encoding-mapping>
</locale-encoding-mapping-list>
```

设置好以上信息后，若使用 `resp.setLocale(Locale.TAIWAN)`，或者 `resp.setLocale(new Locale("zh", "TW"))`，那么 `HttServletResponse` 的字符编码就是 UTF-8，`getCharacterEncoding()` 返回的结果就是 UTF-8。

**使用 setCharacterEncoding() 或 setContentType()**

使用 `setContentType()` 时，指定的 charset 值会自动用来调用 `setCharacterEncoding()`。

如果使用了 `setCharacterEncoding()` 或 `setContentType()`（指定了 charset），则自动忽略 `setLocale()` 方法。

```java
response.setCharacterEncoding("UTF-8");
```

```java
response.setContentType("text/html; charset=UTF-8");
```