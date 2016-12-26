# Thymeleaf

**Thymeleaf** 是一个现代服务器端的 Java 模板引擎，在 Web 和独立环境下都可以使用它。

Thymeleaf 的主要目标是为你的开发工作流程提供优雅的*自然模板（natural templates）* - HTML 可以在浏览器中正确地显示，也可以作为静态原型，允许开发团队具有更有力的协作。

Thymeleaf 使用了 Spring Framework 的模块，以多种方式集成你喜欢的工具，还具有插入你自己的功能的能力，还有其他更多的功能，它 是现代的 HTML 5 JVM Web 开发的理想选择。

## 自然模板

在 Thymeleaf 中编写的 HTML 模板看起来和工作起来仍然像 HTML 一样，这让在应用程序中运行的实际模板仍然可以作为有用的设计构件。

````html
<table>
  <thead>
    <tr>
      <th th:text="#{msgs.headers.name}">Name</th>
      <th th:text="#{msgs.headers.price}">Price</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="prod: ${allProducts}">
      <td th:text="${prod.name}">Oranges</td>
      <td th:text="${#numbers.formatDecimal(prod.price, 1, 2)}">0.99</td>
    </tr>
  </tbody>
</table>
````

## 丰富的集成

Eclipse、IntelliJ IDEA、Spring、Play，甚至即将推出的用于 Java EE 8 的 Model-View-Controller API。你可以使用喜欢的 Web 开发框架在最喜欢的工具中编写 Thymeleaf。

查看我们的 [生态系统](http://www.thymeleaf.org/ecosystem.html) 以查看更多集成，包括社区编写的用于加快开发 Thymelea 的插件。

## 开始

想要开始吗？ 查看我们的 [下载](http://www.thymeleaf.org/download.html) 部分以获得 Thymeleaf，然后去我们的 [文档](http://www.thymeleaf.org/documentation.html) 页面看看几个教程，就可以慢慢上手 Thymeleaf 了。

发现了一个错误，或想要贡献？可以在我们的 [问题跟踪](http://www.thymeleaf.org/issuetracking.html) 页面上找到 GitHub 仓库。

卡住了？有问题吗？ 可以在 [论坛](http://forum.thymeleaf.org/)上找到我们并提问。

## 谁在使用 Thymeleaf？

[很多人都是](http://www.thymeleaf.org/whoisusingthymeleaf.html):)一些人非常友善地提供给我们相关的内容，他们如何找到 Thymeleaf 以及使用它的方式。点击并阅读他们所说的内容。