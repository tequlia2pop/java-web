# Web 应用程序基础

## HTML

HTML 以标签（Tag）的方式来定义文件结构。

[w3schools 的 HTML Tutorial](http:www.w3schools.com/html/default.asp) 是不错的入门文件。

## URL、URN 与 URI

* *URL, Uniform Resource Locator* 统一资源定位符，代表资源的地址信息
* *URN, Uniform Resource Name* 统一资源名称，代表某个资源独一无二的名称。例如，国际标准书号（International Standard Book Number, ISBN） 就是 URN 的一个例子。
* *URI, Uniform Resource Identifier* 统一资源标识符。URL 和 URN 都是 URI 的子集。

URL 的主要格式：

```
protocol://[host.]domain[:port][/context][/resource][?query string]
```

```
protocol://IP address[:port][/context][/resource][?query string]
```

协议（protocol）指定了以何种方式取得资源：

* http（Hypertext Transfer Protocol，超文本传输协议）
* ftp（File Transfer Protocol，文件传输协议）
* mailto（电子邮件）
* file（特定主机文件名）

## HTTP

P18

HTTP 是一种基于请求/响应的通信协议。客户端对服务器发出一个请求，服务器将要求的资源响应给客户端，每次的联机只作一次请求/响应，没有请求就没有响应。

HTTP 是一种无状态的通信协议。具体来说，服务器响应客户端之后，就不会记得客户端的信息，更不会去维护与客户端有关的状态。

**GET & POST**

根据操作是否是等幂操作来决定应该使用 GET 还是 POST。GET 应用于等幂操作的请求，POST 应用于非等幂的操作的请求。

GET 和 POST 的差别还在于 URL 数据长度的限制，以及是否在地址栏上出现请求参数。

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

## 动态网页和静态网页

静态网页：请求服务器上的网页时，服务器不对网页文件做任何处理，读取文件之后就直接当作响应传给浏览器。
动态网页：服务器在响应之前，可能先依赖客户端的请求参数、标头或实际服务器上的状态，以程序的方式动态产生响应内容，再传回给用户。

处理动态网页的技术：

* CGI（Common Gateway Interface）
* PHP（Hypertext Preprocessor）
* ASP（Active Server Pages）
* Servlet/JSP(JavaServer Pages）

