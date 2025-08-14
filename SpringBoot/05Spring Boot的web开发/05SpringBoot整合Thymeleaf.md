
## 传统web应用和前后端分离
如果你是做前后端分离的项目，这一章节的内容将用不上。

现代开发大部分应用都会采用前后端分离的方式进行开发，前端是一个独立的系统，后端也是一个独立的系统，后端系统只给前端系统提供数据（JSON数据），不需要后端解析模板页面，前端系统拿到后端提供的数据之后，前端负责填充数据即可。因此这一章节内容作为了解。



传统的WEB应用（非前后端分离）：浏览器页面上展示成什么效果，后端服务器说了算，这是传统web应用最大的特点。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730789733440-1dc2d749-19c5-4363-b35d-bf6e6008265c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

前后端分离的应用：前端是一个独立的系统，后端也是一个独立的系统，后端系统不再负责页面的渲染，后端系统只负责给前端系统提供开放的API接口，后端系统只负责数据的收集，然后将数据以JSON/XML等格式响应给前端系统。前端系统拿到接口返回的数据后，将数据填充到页面上。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730790760614-212e3911-eb37-4dca-88a4-eca6ecd26dba.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

前后端分离的好处：

+ 职责清晰：前端专注于用户界面和用户体验，后端专注于业务逻辑和数据处理。
+ 开发效率高：前后端可以并行开发，互不影响，提高开发速度。
+ 可维护性强：代码结构更清晰，便于维护和扩展。
+ 技术栈灵活：前后端可以独立选择最适合的技术栈。
+ 响应式设计：前端可以更好地处理不同设备和屏幕尺寸。
+ 性能优化：前后端可以独立优化，提升整体性能。
+ 易于测试：前后端接口明确，便于单元测试和集成测试。



## SpringBoot整合Thymeleaf
Java的模板技术有很多，SpringBoot支持以下的模板技术：

1. **Thymeleaf**：
    - **特点**：Thymeleaf 是一个现代的服务器端Java模板引擎，它支持HTML5，XML，TEXT，JAVASCRIPT，CSS等多种模板类型。它能够在浏览器中预览，这使得前端开发更加便捷。Thymeleaf 提供了一套强大的表达式语言，可以轻松地处理数据绑定、条件判断、循环等。
    - **优势**：**<font style="color:#DF2A3F;">与Spring框架集成良好，也是SpringBoot官方推荐的</font>**。
2. **FreeMarker**：
    - **特点**：FreeMarker 是一个用Java编写的模板引擎，主要用来生成文本输出，如HTML网页、邮件、配置文件等。它不依赖于Servlet容器，可以在任何环境中运行。
    - **优势**：模板语法丰富，灵活性高，支持宏和函数定义，非常适合需要大量定制化的项目。
3. **Velocity**：
    - **特点**：Velocity 是另一个强大的模板引擎，最初设计用于与Java一起工作，但也可以与其他技术结合使用。它提供了简洁的模板语言，易于学习和使用。
    - **优势**：轻量级，性能优秀，特别适合需要快速生成静态内容的应用。
4. **Mustache**：
    - **特点**：Mustache 是一种逻辑无感知的模板语言，可以用于多种编程语言，包括Java。它的设计理念是让模板保持简单，避免模板中出现复杂的逻辑。
    - **优势**：逻辑无感知，确保模板的简洁性和可维护性，易于与前后端开发人员协作。
5. **Groovy Templates**：
    - **特点**：Groovy 是一种基于JVM的动态语言，它可以作为模板引擎使用。Groovy Templates 提供了非常灵活的模板编写方式，可以直接嵌入Groovy代码。
    - **优势**：对于熟悉Groovy语言的开发者来说，使用起来非常方便，可以快速实现复杂逻辑。

这些模板技术各有千秋，选择哪一种取决于项目的具体需求和个人偏好。Spring Boot 对这些模板引擎都提供了良好的支持，通常只需要在项目中添加相应的依赖，然后按照官方文档配置即可开始使用。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



<font style="color:#DF2A3F;">提醒：SpringBoot内嵌了Servlet容器（例如：Tomcat、Jetty等），使用SpringBoot不太适合使用JSP模板技术，因为SpringBoot项目最终打成jar包之后，放在jar包中的jsp文件不能被Servlet容器解析。</font>



要在SpringBoot中整合Thymeleaf，按照以下步骤操作：

第一步：引入thymeleaf启动器

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

第二步：编写配置文件，指定前缀和后缀（**<font style="color:#DF2A3F;">默认不配置就是以下配置</font>**）

```properties
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

第三步：编写控制器

```java
package com.powernode.springboot.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

// 不能使用 @RestController
@Controller
public class HelloController {

    @GetMapping("/h")
    public String helloThymeleaf(@RequestParam("name") String name, Model model) {
        // 将接收到的name数据存储到域对象中
        model.addAttribute("name", name);
        // 逻辑视图名
        return "hello"; // 最终的物理视图名：classpath:/templates/hello.html
    }
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

第四步：编写thymeleaf模板页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>hello, thymeleaf</title>
</head>
<body>
<h1>hello,<span th:text="${name}"></span></h1>
</body>
</html>
```

启动服务器，测试地址为：http://localhost:8080/h

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792132534-7b9358f0-98ff-43dc-8624-1717683b15aa.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## Thymeleaf的自动配置
Thymeleaf的自动配置类：`ThymeleafAutoConfiguration`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792250202-195d729c-66e5-46e9-b86e-6cf9494ef895.png)

我们通过查看上图中红框中的类，可以找到thymeleaf的相关配置：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792335100-efa4c9cf-96c4-456d-92f6-71ceaf7f9452.png)

与thymeleaf相关的配置统一使用`spring.thymeleaf`即可。并且通过源码可以进一步了解到默认的前缀和后缀：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792465391-d516ec42-a8da-46f4-8325-197617074a6a.png)

也就是说，默认情况下，只要放在`classpath:/templates/`目录下的`xxx.html`会被自动当做为thymeleaf的模板文件被thymeleaf模板引擎解析。因此放在`classpath:/templates/`目录下的`html`文件不是静态页面，而是动态的thymeleaf模板页面。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## Thymeleaf核心语法
### th:text 替换标签体内容
注意：在根标签<html>中引入 xmlns:th="[http://www.thymeleaf.org"](http://www.thymeleaf.org")，在编写`th:`语法时有智能提示。

`th:text`用来替换标签体内容的，例如：

```html
th:text 语法：替换标签体中的内容
<div th:text="${name}">我是一个DIV</div>
```

运行效果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730794669882-1e70cf45-36bf-456f-97a1-6b1fb3b45f30.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



提示：标签体中的内容即使是一段HTML代码，也只是会被当做普通文本对待。例如我们让存储在域中的文本内容是一段HTML代码：

```java
model.addAttribute("htmlCode", "<a href='http://www.bjpowernode.com'>动力节点</a>");
```

```html
<div th:text="${htmlCode}">我是一个DIV</div>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730795096806-e6cf0de5-6295-4e67-990b-97a9ace01976.png)

### th:utext 替换标签体内容
作用和 `th:text`一样。只不过`th:utext`会将内容当做一段HTML代码解析并替换。将以上测试代码修改为：

```html
th:utext 语法：替换标签体中的内容
<div th:utext="${htmlCode}">我是一个DIV</div>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730795321767-2f90cc02-87da-4269-aae1-26bb3fa0f939.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### th:任意属性名 动态替换该属性的值
例如：我们向域中存储以下数据

```java
// 向域中存储一个html标签的某个属性的值
model.addAttribute("company", "动力节点");
model.addAttribute("hrefValue", "http://www.bjpowernode.com");
```

然后使用`th:href`动态替换`href`属性的值：

```html
th:任意属性名 语法：动态替换属性值
<a th:href="${hrefValue}" href="https://www.baidu.com" th:text="${company}">百度</a>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730795806734-d453db1e-86d3-4ecf-affb-cdff95491241.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### th:attr 属性合并设置
+ 分开设置：

```java
model.addAttribute("hrefValue", "http://www.bjpowernode.com");
model.addAttribute("style", "color:red");
```

```html
<a th:href="${hrefValue}" th:style="${style}">动力节点</a>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730796198283-c5a66c03-144d-4338-8d86-4b252747bc1f.png)

+ 合并设置：使用`th:attr`

```java
model.addAttribute("hrefValue", "http://www.bjpowernode.com");
model.addAttribute("style", "color:red");
```

```html
<a th:attr="href=${hrefValue},style=${style}">动力节点</a>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### th:指令
指令非常多，具有代表性的例如：`th:if`，该指令用来控制元素`隐藏`和`显示`。

在`static`静态资源目录下存放一张图片：dog1.jpg

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730796605419-b6da6632-95e2-4b77-bb64-e64cc1a50e06.png)

然后编写这样的模板代码：

```html
<img src="dog1.jpg" th:if="true">
```

测试结果，图片是显示的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730796706035-255d756e-2806-4f41-8ef8-83c1412ae091.png)

如果`th:if`的值修改为`false`，我们会发现隐藏了。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### @{} 表达式
`${}`表达式语法是专门用来获取`model`中绑定的数据的。

`@{}`表达式语法是专门用来维护URL请求路径的。它可以动态设置项目的根路径。



SpringBoot中默认的项目根路径是：`/`

假设我们编写这样的java代码，向model中绑定一个路径：

```java
model.addAttribute("imgUrl", "/dog1.jpg");
```

我们编写这样的模板代码：

```html
<img th:src="${imgUrl}">
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730797446050-1a8e29e1-6339-4311-8420-73dee00a3d3d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

此时是可以正常显示的，但如果我们将web应用的根路径进行了修改，将其配置为：`/myweb`

```properties
server.servlet.context-path=/myweb
```

其他位置代码不做修改，我们再来访问页面，注意访问路径中要添加`/myweb`，例如：[http://localhost:8080/myweb/h?name=jackson](http://localhost:8080/myweb/h?name=jackson)

访问后使用`ctrl + F5`强行刷新，不走浏览器缓存，结果发现无法访问到该图片：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730797689412-190d1f6c-7cb2-42c0-9414-30aa6550e699.png)



此时我们将模板代码进行如下修改，将`${}`修改为`@{}`：

```html
<img th:src="@{${imgUrl}}">
```

再次测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730797801499-a27a88b1-9024-4e0f-90fe-d2a64440d41c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### thymeleaf的内置工具
内置工具很多，可以参考官方文档：[https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#strings](https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#strings)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798338220-625af768-8576-4d24-a89d-46eaacef936c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

例如我们要完成这样一个效果，用户提交用户名时，如果名字中含有`jack`则显示`狗狗图片`，否则不显示。

模板代码如下：

```html
<img th:src="@{${imgUrl}}" th:if="${#strings.contains(name,'jack')}">
```

测试结果1：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798493853-7e6e9545-f7dd-43b3-b157-7c853fb2da66.png)

测试结果2：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798509202-cb67e7fe-f8ab-487e-ba9e-470ab50dcfbf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### thymeleaf也支持运算符
例如`>`运算符，我们实现这样一个功能，如果提供的用户名长度大于6，则显示狗狗图片：

```html
<img th:src="@{${imgUrl}}" th:if="${#strings.length(name) > 6}">
```

测试结果1：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798722013-6be35719-74db-4349-86d9-d5c8b99f0d5b.png)

测试结果2：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798741960-7e1c643d-9ac1-4796-972e-b61f23b988d4.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

另外thymeleaf也支持三目运算符：

```html
<span th:text="${user.gender ? '男' : '女'}"></span>
```

如果性别是true，则显示男，false，则显示女。



还有很多其他的运算符，可参考thymeleaf官方文档。



### thymeleaf的字符串拼接
第一种方式：使用加号 `+`

```html
<div th:text="${'姓名：' + name + '，年龄：18'}"></div>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798992476-8176fe7d-332f-4806-be1a-84923463232e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

第二种方式：使用竖线 `||`

```html
<div th:text="|姓名：${name}，年龄：18|"></div>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730799059381-eb2cc823-306a-4f52-9ae5-0012783c8f14.png)

### 循环遍历
代码示例：

```html
<tr th:each="user : ${users}">
  <td th:text="${user.name}"></td>
  <td th:text="${user.age}"></td>
  <td th:text="${user.gender}"></td>
  <td th:text="${user.desc}"></td>
  <td th:text="${user.location}"></td>
</tr>
```

说明：

+ ${users}：代表集合
+ user：代表集合中的每个元素
+ ${user.name}：元素的name属性值

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

遍历时也可以添加状态对象，代码示例：

```html
<tr th:each="user,state : ${userList}">
    <td th:text="${state.count}">1</td>
    <td th:text="${user.name}">张三</td>
    <td th:text="${user.age}">20</td>
    <td th:text="${user.gender}"></td>
    <td th:text="${user.desc}"></td>
    <td th:text="${user.location}"></td>
    <td>
        thymeleaf的内联表达式：<br>
        下标：[[${state.index}]]<br>
        序号：[[${state.count}]]<br>
        当前对象：[[${state.current}]]<br>
        元素总数：[[${state.size}]]<br>
        是否为偶数行：[[${state.even}]]<br>
        是否为奇数行：[[${state.odd}]]<br>
        是否第一个元素：[[${state.first}]]<br>
        是否最后一个元素：[[${state.last}]]<br>
    </td>
</tr>
```



注意：以上`[[${state.index}]]`这种语法属于thymeleaf中的`内联表达式`写法。也可以写成：`[(${state.index})]`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

另外，状态对象`state`的属性包括：

+ index：下标
+ count：序号
+ current：当前对象
+ size：元素总数
+ even：是否为偶数行
+ odd：是否为奇数行
+ first：是否为第一个元素
+ last：是否为最后一个元素

### 条件判断th:if
th:if 语法用来决定元素是否显示：true显示。false隐藏。

<div th:if="true">我是一个div元素</div>，则显示该div

<div th:if="false">我也是一个div元素</div>，则隐藏该div



实现这样一个功能：用户如果没有留下简介，则显示`你比较懒没有留下任何介绍信息`，如果留下了简介，则显示具体的简介信息。

```html
<td th:if="${#strings.isEmpty(user.desc)}" th:text="'你比较懒没有留下任何介绍信息'"></td>
<td th:if="${not #strings.isEmpty(user.desc)}" th:text="${user.desc}"></td>
```

### 条件判断th:switch
实现一个这样的功能：如果城市编号001则显示北京，002则显示上海，003则显示广州，004则显示深圳，其他值显示未知。

```html
<td th:switch="${user.location}">
  <span th:case="001">北京</span>
  <span th:case="002">上海</span>
  <span th:case="003">广州</span>
  <span th:case="004">深圳</span>
  <span th:case="*">未知</span>
</td>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### thymeleaf属性优先级
thymeleaf的属性优先级非常重要，因为它直接决定了模板的解析和执行顺序。

以下是Thymeleaf属性的优先级从高到低的列表，以表格形式展示：

| 优先级 | 属性 | 描述 |
| --- | --- | --- |
| 1 | `th:if` | 如果条件为真，则渲染该元素。 |
| 2 | `th:unless` | 如果条件为假，则渲染该元素。 |
| 3 | `th:with` | 定义局部变量。 |
| 4 | `th:switch` | 开始一个 switch 语句。 |
| 5 | `th:case` | 定义 switch 语句中的 case 分支。 |
| 6 | `th:each` | 遍历列表，用于循环。 |
| 7 | `th:remove` | 移除元素或其属性。 |
| 8 | `th:attr` | 设置或修改元素的属性。 |
| 9 | `th:classappend` | 追加 CSS 类。 |
| 10 | `th:styleappend` | 追加内联样式。 |
| 11 | `th:src` | 设置元素的 `src` 属性。 |
| 12 | `th:href` | 设置元素的 `href` 属性。 |
| 13 | `th:value` | 设置元素的 `value` 属性。 |
| 14 | `th:text` | 设置元素的文本内容。 |
| 15 | `th:utext` | 设置元素的未转义文本内容。 |
| 16 | `th:html` | 设置元素的 HTML 内容。 |
| 17 | `th:fragment` | 定义模板片段。 |
| 18 | `th:insert` | 插入一个模板片段。 |
| 19 | `th:replace` | 替换当前元素为一个模板片段。 |
| 20 | `th:include` | 包含一个模板片段的内容。 |
| 21 | `th:block` | 用于逻辑分组，不产生任何HTML输出。 |


![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

对于thymeleaf属性优先级，我总结了以下一段话，把它记住即可：

**“先控制，再遍历，后操作，末内容。”**

具体来说：

1. **先控制**：`th:if` 和 `th:unless` 用于条件控制，决定是否渲染元素。
2. **再遍历**：`th:each` 用于遍历列表，生成多个元素。
3. **后操作**：`th:with`、`th:switch`、`th:case`、`th:remove`、`th:attr` 等用于局部变量定义、条件分支、属性操作等。
4. **末内容**：`th:text`、`th:utext`、`th:html` 等用于设置元素的内容。

### *{...} 表达式
`*{...}` 主要用于在上下文中访问对象的属性。这种表达式通常在表单处理和对象绑定场景中使用。



**语法**：`*{property}`：访问当前**<font style="color:#DF2A3F;">上下文</font>**对象的某个属性。



**使用场景**

+ 表单绑定：在表单中绑定对象的属性。
+ 对象属性访问：在模板中访问对象的属性，特别是当对象是当前上下文的一部分时。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**示例**

1. **表单绑定**

假设你有一个 `User` 对象，包含 `name` 和 `age` 属性，你可以在表单中使用 `*{...}` 表达式来绑定这些属性：

```html
<form th:object="${user}" method="post" action="/submit">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" th:field="*{name}" /> 
    <label for="age">Age:</label>
    <input type="number" id="age" name="age" th:field="*{age}" />
    <button type="submit">Submit</button>
</form>
```

在这个例子中：

+ `th:object="${user}"` 将 `user` 对象设置为当前上下文对象。
+ `th:field="*{name}"` 和 `th:field="*{age}"` 分别绑定到 `user` 对象的 `name` 和 `age` 属性。



2. **对象属性访问**

假设你有一个 `User` 对象，包含 `name` 和 `age` 属性，你可以在模板中使用 `*{...}` 表达式来访问这些属性：

```html
<div th:object="${user}">
    <p>Name: <span th:text="*{name}">Default Name</span></p>
    <p>Age: <span th:text="*{age}">Default Age</span></p>
</div>

```

在这个例子中：

+ `th:object="${user}"` 将 `user` 对象设置为当前上下文对象。
+ `*{name}` 和 `*{age}` 分别访问 `user` 对象的 `name` 和 `age` 属性。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**与 **`**${...}**`** 的区别**

+ `${...}`：标准表达式，用于访问模型中的变量和执行简单的表达式。
+ `*{...}`：属性选择表达式，用于在上下文中访问对象的属性，通常与 `th:object` 一起使用。

### 代码片段共享
片段是Thymeleaf中用于代码复用的基本机制。你可以将共享的部分提取到单独的HTML文件中，然后在其他模板中引用这些片段。



页面中公共的header.html

```html
<header th:fragment="h">
    <nav>
        <ul>
            <li><a th:href="@{/a}">Home</a></li>
            <li><a th:href="@{/b}">About</a></li>
        </ul>
    </nav>
</header>
```

页面中公共的footer.html

```html
<footer th:fragment="f">
  <p>&copy; 2024 北京动力节点</p>
</footer>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

在`a.html`中包含以上两个公共部分：

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>主页</title>
</head>
<body>
<div th:replace="~{header :: h}"></div>

<main>
    <h1>欢迎来到主页</h1>
    <p>这是主页的主要内容.</p>
</main>

<div th:replace="~{footer :: f}"></div>
</body>
</html>
```

在`b.html`中包含以上两个公共部分：

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>关于我们</title>
</head>
<body>

<div th:replace="~{header :: h}"></div>

<main>
    <h1>关于我们</h1>
    <p>动力节点专注IT培训16年.</p>
</main>

<div th:replace="~{footer :: f}"></div>
</body>
</html>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

主要作用是代码复用。实现此功能的主要代码：

+ 在公共代码部分使用`th:fragment="片段名称"`来声明公共代码片段的名字。
+ 在需要引入的地方使用`th:replace="~{文件名去掉后缀 :: 片段名称}"`来引入。



**<font style="color:#DF2A3F;">小插曲：在springboot中如何实现：直接将请求路径映射到特定的视图，而不需要编写controller？</font>**

+ 第一步：视图解析器配置

```properties
spring.mvc.view.prefix=/templates/
spring.mvc.view.suffix=.html
```

+ 第二步：使用`ViewControllerRegistry`进行视图与控制器的注册

```java
package com.powernode.springboot.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/a").setViewName("a");
        registry.addViewController("/b").setViewName("b");
    }
}

```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## thymeleaf页面修改如何立即生效
第一步：引入springboot提供的`dev tools`

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
  <scope>runtime</scope>
  <optional>true</optional>
</dependency>
```

第二步：关闭应用重启功能，如果不关闭会导致每一次修改java代码后立即重启应用，不建议。我们现在只希望做到的功能是，修改thymeleaf模板文件后立即生效。

```properties
spring.devtools.restart.enabled=false
```

第三步：修改代码后在IDEA中按组合键`ctrl+f9`



以上三步配合即可。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)
