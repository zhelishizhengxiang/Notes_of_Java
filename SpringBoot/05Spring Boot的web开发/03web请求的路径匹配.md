
我们在学习SpringMVC的时候，路径匹配规则中学习了`Ant`风格的路径匹配规则。大家可以翻看一下之前的Spring MVC视频。

在`Spring Boot3`中，对**web请求的模糊路径匹配提供了两种规则**：

+ 第一种：AntPathMatcher（Ant风格）【较旧**】
+ **第二种：PathPatternParser（从Spring5.3中引入的。在SpringBoot2.4中引入的**。）【**<font style="color:#DF2A3F;">较新：效率高</font>**】
    - **效率比Ant高，一般新项目中使用 PathPatternParser**



**SpringBoot3中默认使用的是`PathPatternParser`，不需要任何配置**。如果要使用`AntPathMatcher`，就需要进行如下的配置：

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

## 1.AntPathMatcher
Ant风格的路径匹配规则回顾：

**<font style="color:#DF2A3F;">*</font>**	**匹配任意长度的任意字符序列（不包括路径分隔符）**。示例：/foo/\*.html 匹配 /foo/bar.html 和 /foo/baz.html。

**<font style="color:#DF2A3F;">**</font>**	**匹配任意数量的目录层级**。示例：/foo/** 匹配 /foo/bar、/foo/bar/baz 和 /foo/bar/baz/qux。

**<font style="color:#DF2A3F;">?</font>****	**匹配任意单个字符**。示例：/foo?bar 匹配 /foobar 和 /fooxbar。

**<font style="color:#DF2A3F;">[]</font>****	**匹配指定范围内的单个字符。示例：/foo[a-z]bar 匹配 /fooabar、/foobbar 等。

**<font style="color:#DF2A3F;">{}</font>****	**路径变量，用于提取路径的一部分作为参数。示例：/users/{userId} 匹配 /users/123，提取 userId=123。



如果在SpringBoot3中启用Ant风格，记得配置：

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

如下代码：请分析以下路径匹配的是什么样的路径。

```java
package com.powernode.springboot.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PathController {

    @GetMapping("/{path:[a-z]+}/a?/**/*.do")
    public String path(HttpServletRequest request, @PathVariable String path){
        return request.getRequestURI() + "," + path;
    }
}
```
* [a-z]+代表a-z个1-n个字符，[a-z]代表a-z的1个字符，上图表示用户传进来的路径会满足该要求，并且会赋值给path变量


启动服务器并测试路径：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730472107928-af151fcc-d9b2-439a-825e-660f87f2c952.png)



## 2.PathPatternParser
项目中不做配置，或者按照以下方式配置，都是`PathPatternParser`：

```properties
spring.mvc.pathmatch.matching-strategy=path_pattern_parser
```

**`PathPatternParser`风格是兼容Ant风格的。只有一个地方`PathPatternParser`不支持**，Ant支持。在Ant风格中，**`**`可以出现在任意位置。在`PathPatternParser`中只允许`**`出现在路径的末尾**。



可以测试一下，将配置文件中的Ant风格注释掉，采用`PathPatternParser`风格。然后控制器代码如下：

```java
package com.powernode.springboot.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PathController {

    @GetMapping("/{path:[a-z]+}/a?/**/*.do")
    public String path(HttpServletRequest request, @PathVariable String path){
        return request.getRequestURI() + "," + path;
    }
}
```

启动服务器报错：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730472557923-8f34cf00-6454-465b-b5e8-562520bbf095.png)

提示你，如果在路径当中出现了`**`，需要将路径匹配规则替换为Ant风格。因此路径当中如果出现`**`，那么必须使用Ant风格。除此之外，`PathPatternParser`均可用。



我们再来测试一下，`**`放到末尾，对于`PathPatternParser`是否可用？

```java
@GetMapping("/{path:[a-z]+}/a?/*.do/**")
public String path(HttpServletRequest request, @PathVariable String path){
    return request.getRequestURI() + "," + path;
}
```

启动服务器测试，可用：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730472772231-d18e628f-1b62-4757-9299-eaa604344ce6.png)



## 3.路径匹配相关源码
底层选择路径匹配规则的源码是：

![1730451084171-157f7728-46f8-4715-8866-dcb1e9215206.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730451084171-157f7728-46f8-4715-8866-dcb1e9215206.png)
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730451084171-157f7728-46f8-4715-8866-dcb1e9215206.png)

