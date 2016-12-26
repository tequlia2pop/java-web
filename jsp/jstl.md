# JSP 标准标签库（JavaServer Pages Standard Tag Library）

## 核心

### 变量支持

#### `<c:set>`

`<c:set>` 标签是 `setProperty` 操作的 JSTL 友好（JSTL-friendly）的版本。 该标签是有帮助的，因为它会计算表达式并使用结果设置 `JavaBean` 或 `java.util.Map` 对象的值。

**属性**

`<c:set>` 标签具有下列属性：

属性 | 描述 | 必需 | 默认值
--- | ---- | --- | ------
value | 要保存的数据 | No | body
target | 应修改其属性的变量的名称 | No | 无
property | 要修改的属性 | No | 无
var | 保存数据的变量的名称 | No | 无
scope | 保存数据的变量的作用域 | No | page

### 流程控制

### `<c:forEach>`、`<c:forTokens>`

为了执行流程控制，可以在 scriptlet 中嵌入一段 Java 的 `for`、 `while` 或 `do-while` 循环。这些标签的出现是上述方式的一种很好的替代。`<c:forEach>` 标签用于遍历对象集合，所以它是更为常用的。`<c:forTokens>` 标签用于将字符串拆分为 token 并遍历每个 token。

**属性**

`<c:forEach>` 标签具有下列属性：

属性 | 描述 | 必需 | 默认值
--- | ---- | --- | ------
items |	要遍历的数据 | No | 无
begin |	从哪个元素开始遍历（0 = 第一个项，1 = 第二项，……） | No | 0
end | 在哪个元素停止遍历（0 = 第一个项，1 = 第二项，……） | No | 最后一个元素
step | 每一次处理多少项 | No | 1
var | 用于显示当前项的变量名称 | No | 无
varStatus | 用于显示遍历状态的变量名称 | No | 无

`<c:forTokens>` 标签具有与 `<c:forEach>` 类似的属性，另外增加了一个属性 `delims`，它指定用作分隔符的字符。

`varStatus` 属性是一个 `javax.servlet.jsp.jstl.core.LoopTagStatus` 实例，用于封装遍历的状态，可以帮助实现一些与行相关的功能，如判断奇数或偶数行、对最后一行进行特殊处理等等。

`varStatus` 具有下列属性：

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

#### `<c:if>`

`<c:if>` 标签计算表达式并仅在表达式计算结果为 `true` 时显示其正文内容。

**属性**

`<c:if>` 标签具有下列属性：

属性 | 描述 | 必需 | 默认值
--- | ---- | --- | ------
test | 评估条件 | Yes | 无
var | 存储条件结果的变量名称 | No | 无
scope | 存储条件结果的变量所处的作用域 | No | page

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

#### `<c:out>`

`<c:out>` 标签用于显示表达式的结果，类似于 `<％=％>`。 在 `<c:out>` 标签中可以使用更简单的 "." 符号来访问属性。 例如，要访问 `customer.address.street`，使用的标记为 `<c:out value ="customer.address.street"/>`。

`<c:out>` 标签可以自动对 XML 标签进行转义。

**属性**

`<c:out>` 标签具有下列属性：

属性 | 描述 | 必需 | 默认值
--- | ---- | --- | ------
value | 要输出的数据 | Yes | 无
default | 要输出的默认数据 | No | body
escapeXml | 如果标签应该对特殊的 XML 字符进行转移，则设置为 true | No | true

## XML

## 国际化

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

### 格式化标签

## 数据库

## 函数


