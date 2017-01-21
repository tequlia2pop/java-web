# JSTL

JSTL（JavaServer Pages Standard Tag Library，JSP 标准标签库）是一个定制标签类库的集合。

JSTL 由 JCP（www.jcp.org）下的 JSR-52 专家组定义。下载地址为 http://jstl.java.net

JSTL 1.2 中的标签由五大类组成：

类别 | 下属功能 | URI | 前缀
--- | ------- | --- | ----
Core | 变量支持<br/>流程控制<br/>URL 管理<br/>杂项 | http://java.sun.com/jsp/jstl/core | c
XML | Core<br/>流程控制<br/>转换 | http://java.sun.com/jsp/jstl/xml | x
I18n | 语言环境<br/>消息格式化<br/>数字和日期格式化 | http://java.sun.com/jsp/jstl/fmt | fmt
数据库 | SQL | http://java.sun.com/jsp/jstl/sql | sql
函数 | 集合长度<br/>字符串操作 | http://java.sun.com/jsp/jstl/functions | fn

为了在 JSP 中使用 JSTL 类库，必须使用 taglib 指令引用 JSTL 类库：

```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

注意：标签属性的 rtexprvalue（runtime expression value）表示属性是否接受运行时运算的结果。若值为 true，表示该属性值可以为静态字符串或者动态值（Java 表达式、EL 表达式，或者用 `<jsp:attribute>` 设置的值）。若值为 false，表示该属性的值只能为静态字符串。

提示：标签属性的名称后面加星号（*）表示该属性是必需的，加号（+）表示该属性的 rtexprvalue 值为 true。

## Core

### 变量支持

#### set

set 标签是 `setProperty` 动作的 JSTL 友好（JSTL-friendly）版本。

set 标签可以完成以下工作：

* 创建一个限域对象（scoped variable），并引用一个已经存在的限域对象。
* 设置限域对象的属性值。

set 标签的语法有四种形式：

```
<c:set value="value" var="varName" [scope="{*page*|request|session|application}"] />
```
```
<c:set var="varName" [scope="{*page*|request|session|application}"] >
	body content
</c:set>
```
```
<c:set target="target" property="propertyName" value="value" />
```
```
<c:set target="target" property="propertyName">
	body content
</c:set>
```

set 的属性列表如下：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
value+ | Object | 要创建的字符串，或要引用的限域对象，或新的属性值 | N | true | body
var | String | 要创建的限域对象 | N | false | 无
scope | String | 新建限域对象的范围 | N | false | page
target+ | Object | 要修改属性的限域对象的名称，JavaBean 或 java.util.Map | N | true | 无
property+ | String | 要修改的属性名称 | N | true | 无

#### remove

remvoe 标签用于删除限域对象（scoped variable）。

remvoe 的语法为：

```
<c:remove var="varName" [scope="{*page*|request|session|application}"] />
```

remove 的属性列表如下：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
var | String | 要删除的限域对象的名称 | Y | false | 无
scope | String | 要删除的限域对象的范围 | Y | false | page

### 流程控制

#### if

if 标签先对某个条件进行测试，如果该条件的计算结果为 `true` 则处理它的主体内容。测试结果保存在一个 Boolean 对象中，并创建一个限域变量来引用这个 Boolean 对象。

if 的语法有两种形式：

```
<c:if test="testCondition" var="varName" [scope="{*page*|request|session|application}"] />
```

```
<c:if test="testCondition" [var="varName"] [scope="{*page*|request|session|application}"] >
	body content
</c:if>
```

if 的属性列表如下：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
test+ | Boolean | 测试条件 | Y | true | 无
var | String | 引用条件结果的限域变量名称，var 的类型为 Boolean | N | false | 无
scope | String | var 设置的限域变量的范围 | No | false | page

为了模拟 else 的情景，需要使用两个 if 标签，并用两个相反的条件。

#### choose、when 和 otherwise

choose 和 when 的作用类似 Java 中的 switch 和 case 关键字。otherwise 标签 用于默认的条件代码块。

choose 和 otherwise 标签没有属性。when 标签则必须使用 test 属性设定一个条件，用于确定是否处理主体内容。

示例如下：

```
<c:choose>
	<c:when test="${param.status == 'full'}">
		You are a full member
	</c:when>
	<c:when test="${param.status == 'student'}">
		You are a student member
	</c:when>
	<c:otherwise>
		Please register
	</c:otherwise>
</c:choose>
```

#### forEach

foreach 用于将主体内容迭代多次，或者迭代一个对象集合。可以迭代的对象包含所有的 `java.util.Collection` 和 `java.util.Map` 接口的实现、对象或基本类型的数组，以及 `java.util.Iterator` 和 `java.util.Enumeration`，但不能在多个动作指令中使用 `Iterator` 或 `Enumeration`，因为 `Iterator` 和 `Enumeration` 都不能重置（reset）。

forEach 的语法有两种形式：

* 将主体内容重复指定的次数。

	```
	<c:foreach [var="varName"] begin="begin" end="end" [step="step"]>
		body content
	</c:foreach>
	```

* 迭代一个对象集合。

	```
	<c:foreach items="collection" [var="varName"] [varStatus="varStatus"] [begin="begin"] [end="end"] [step="step"]>
		body content
	</c:foreach>
	```

forEach 标签的属性列表如下：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
var | String | 引用当前迭代项的限域变量名称 | N | false | 无
items+ | 支持的任何类型 | 要迭代的对象集合 | N | true | 无
varStatus | String | 保存迭代状态的限域变量名称，它的值类型为 `javax.servlet.jsp.jstl.core.LoopTagStatus` | N | false | 无
begin+ | int | 如果指定了 items，那么迭代将从指定索引的项开始（第一项索引为 0）;否则，迭代将从该值设定的索引开始。 | N | true | 0
end+ | int | 如果指定了 items，那么迭代结束于指定索引的项（含）;否则，当迭代达到指定值时，迭代结束。 | N | true | 最后一个元素
step+ | int | 步长 | N | true | 1

foreach 的 `varStatus` 属性是一个 `javax.servlet.jsp.jstl.core.LoopTagStatus` 实例，用于封装遍历的状态，可以帮助实现一些与行相关的功能，如判断奇数或偶数行、对最后一行进行特殊处理等等。

`LoopTagStatus` 对象具有下列可用属性：

属性 | 类型 | 描述
--- | ---- | ---
current | Object | 迭代中的当前项
index | int | 当前这轮迭代的索引（从 0 开始）
count | int | 当前这轮迭代的计数（从 1 开始）
first | boolean | 当前这轮迭代是否为第一次
last | boolean | 当前这轮迭代是否为最后一次
begin | Integer | 关联标签的 begin 属性值
end | Integer | 关联标签的 end 属性值
step | Integer | 关联标签的 step 属性值

#### forTokens

forTokens 标签用于迭代以特定分界符分隔的 token。

forTokens 标签的语法如下：

```
<c:forTokens items="stringOfTokens" delims="delimiters" 
	[var="varName"] [varStatus="varStatus"] [begin="begin"] [end="end"] [step="step"]>
	body content
</c:forTokens>
```

forTokens 标签具有与 forEach 标签类似的属性，只是另外增加了一个属性 `delims`，它指定用作分隔符的字符。

forTokens 标签的属性列表如下：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
var | String | 引用当前迭代项的限域变量名称 | N | false | 无
items+ | 支持的任何类型 | 要迭代的 token 字符串 | N | true | 无
varStatus | String | 保存迭代状态的限域变量名称，它的值类型为 `javax.servlet.jsp.jstl.core.LoopTagStatus` | N | false | 无
begin+ | int | 迭代的起始索引，索引从0开始。 | N | true | 0
end+ | int | 迭代的结束索引，索引从0开始。 | N | true | 最后一个元素
step+ | int | 步长 | N | true | 1
delims+ | String | 一组分界符 | N | true | ...

forTokens 示例如下：

```
<c:forTokens var="item" items="Argentina,Brazil,Chile" delims=",">
	<c:out value="${item}" /><br/>
</c:forTokens>
```

### URL 管理

#### `<c:url>`

`<c:url>` 标签将 URL 格式化为字符串，并将其存储到一个变量中。此标签会在需要的时候自动执行 URL 重写。 `var` 属性指定了一个变量，它代表了格式化后的 URL。

JSTL url 标签只是一种编写 `response.encodeURL()` 方法调用的替代方法。 url 标签提供的唯一真正的优点是适当的 URL 编码，另外还可以由子 `param` 标签指定一些参数。

**属性**

`<c:url>` 标签具有下列属性：

属性 | 描述 | 必需 | 默认值
--- | ---- | --- | ------
value |	基础 URL | Yes | 无
context | / followed by the name of a local web application | No | 当前应用程序
var | 变量的名称，用于暴露要处理的 URL | No | Print to page
scope | 变量的作用域，用于暴露要处理的URL | No | Page

### 杂项

#### out

out 标签用于显示表达式的结果，类似于 JSP 表达式 `<％=％>`。out 的语法有两种形式：

```
<c:out value="value" [escapeXml={*true*|false}]
	[default="defaultValue"] />
```
```
<c:out value="value" [escapeXml={*true*|false}]>
	default value
</c:out>
```

out 的主体内容是 JSP。

out 的属性列表如下：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
value*+ | Object | 要运算的表达式 | Y | true | 无
escapeXml+ | boolean | 表示是否对结果中的特殊字符 <、>、&、' 和 " 进行转义 | N | true | true
default+ | Object | 默认值 | N | true | body

在 JSP 2.0 之后，除非需要将某个值进行字符转换，否则可以转而使用 EL 表达式：`${x}`

## XML

## I18n

### 消息标签

#### `<fmt:message>`

`<fmt:message>` 标签将 key 映射到本地化消息，并执行参数替换。

**属性**

`<fmt:message>` 标签具有下列属性：

属性 | 描述 | 必需 | 默认值
--- | ---- | --- | ------
key | 要查找的消息的 key | No | Body
bundle | 要使用的资源包 | No | 默认的包
var | 要存储本地化消息的变量的名称 | No | Print to page
scope | 要存储本地化消息的变量的作用域 | No | Page

### 消息格式化

### 数字和日期格式化

P114

数字和日期格式化标签的列表如下：

* formatNumber
* formatDate
* timeZone
* setTimeZone
* parseNumber
* parseDate

#### formatNumber

formatNumber 标签用于格式化数字。

formatNumber 标签的语法具有两种形式：

```
<fmt:formatNumber value="numericValue"
		[type="{*number*|currency|percent}"]
		[pattern="customPattern"]
		[currencyCode="currencyCode"]
		[currencySymbol="currencySymbol"]
		[groupingUsed="{true|false}"]
		[maxIntegerDigits="maxIntegerDigits"]
		[minIntegerDigits="minIntegerDigits"]
		[maxFractionDigits="maxFractionDigits"]
		[minFractionDigits="minFractionDigits"]
		[var="varName"]
		[scope="{*page*|request|session|application}"]
/>
```

```
<fmt:formatNumber [type="{*number*|currency|percent}"]
		[pattern="customPattern"]
		[currencyCode="currencyCode"]
		[currencySymbol="currencySymbol"]
		[groupingUsed="{true|false}"]
		[maxIntegerDigits="maxIntegerDigits"]
		[minIntegerDigits="minIntegerDigits"]
		[maxFractionDigits="maxFractionDigits"]
		[minFractionDigits="minFractionDigits"]
		[var="varName"]
		[scope="{*page*|request|session|application}"]>
	numeric value to be fomatted
</fmt:formatNumber>
```

formatNumber 标签的属性如下表：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
value+ | String 或 Number | 要格式化的数值 | N | true | 无
type+ | String | 要格式化为哪种类型：数字、货币或百分比 | N | true | number
pattern+ | String | 定制格式化模式，例如 `.000`、`#,#00.0#` | N | true | 无
currencyCode+ | String | ISO 4217 码 | N | true | 使用浏览器所在地的货币类别
currencySymbol+ | String | 货币符号 | N | true | 无
groupingUsed+ | Boolean | 表明输出中是否包含组分隔符 | N | true | 无
maxIntegerDigits+ | int | 输出的整数部分中最大的数字 | N | true | 无
minIntegerDigits+ | int | 输出的整数部分中最小的数字 | N | true | 无
maxFractionDigits+ | int | 输出的小数部分中最大的数字 | N | true | 无
minFractionDigits+ | int | 输出的小数部分中最小的数字 | N | true | 无 
var | String | 将输出保存为 String 的限域变量名称 | N | false | 无
scope | String | var 的范围 | N | false | 无

ISO 4217 货币代码，如下：

货币类别 | ISO 4217 码 | 最大单位 | 最小单位
------- | ---------- | ------- | -------
加拿大元 | CAD | dollar | cent
人民币 | CNY | yuan | jiao
欧元 | EUR | euro | euro-cent
日元 | JRP | yen | sen
英镑 | GBP | pound | pence
美元 | USD | dollar | cent

#### formatDate

formatDate 标签用于格式化日期。

formatDate 标签的语法如下：

```
<fmt:formatDate value="date"
		[type="{time|date|both}"]
		[dateStyle="{default|short|medium|long|full}"]
		[timeStyle="{default|short|medium|long|full}"]
		[pattern="customPattern"]
		[timeZone="timeZone"]
		[var="varName"]
		[scope="{page|request|session|application}"]
/>
```

formatDate 标签的属性如下表：

属性 | 类型 | 描述 | 必需 | rtexprvalue | 默认值
--- | ---- | --- | ---- | ----------- | -----
value+ | java.utl.Date | 要格式化的日期和/或时间 | ... | true | ...
type+ | String | 要格式化的类型：时间、日期，或时间和日期的组合 | ... | true | ...
dateStyle+ | String | 遵循 java.text.DateFormat 中所定义语义的日期预设格式化的样式
timeStyle+ | String | 遵循 java.text.DateFormat 中所定义语义的时间预设格式化的样式
pattern+ | String | 为格式化定制模式
timeZone+ | String 或 java.util.TimeZone | 表示时间的时区
var | String | 将结果保存为字符串的限域变量名称
scope | String | var 的范围

## 数据库

## 函数

大多数函数都是用于字符串操作的。

函数 | 语法 | 说明
--- | ---- | ---
contains | contains(string, substring) | 检测某个字符串是否包含了指定的子字符串。
containsIgnoreCase | containsIgnoreCase(string, substring) | 检测某个字符串是否包含了指定的子字符串，不区分大小写。
endsWith | endsWith(string, suffix) | 检测某个字符串是否以指定的后缀结尾。
escapeXml | escapeXml(string) | 用于为 String 进行编码，类似使用 out 标签并将 escapeXml 属性设置为 true 时进行的转换。
indexOf | indexOf(string, substring) | 返回在某个字符串中第一次出现指定子字符串时的索引。如果没有找到会返回-1。
join | join(array, separator) | 将一个 String 数组中的所有元素合并为一个字符串，中间用指定的分隔符隔开。
length | length(input) | 返回一个集合中的项目数，或者一个字符串的字符数。
replace | replace(string, beforeSubstring, afterSubstring) | 在 string 中所有的 beforeSubstring 替换为 afterSubstring 并返回结果。
split | split(string, separator) | 将一个字符串分解为一组子字符串，它的作用与 join 相反。
startsWith | startsWith(string, prefix) | 检测某个字符串是否以指定的前缀开头。
substring | substring(string, beginIndex, endIndex) | 返回 string 中从 beginIndex 索引处（含）开始至 endIndex 索引处的子字符串，索引从0开始。
substringAfter | substring(string, substring) | 返回第一次出现指定子字符串之后的字符串部分。
substringBefore | substring(string, substring) | 返回第一次出现指定子字符串之前的字符串部分。
toLowerCase | toLowerCase(string) | 将字符串转换成小写字母。
toUpperCase | toUpperCase(string) | 将字符串转换成大写字母。
trim | trim(string) | 去掉字符串前后的空格。