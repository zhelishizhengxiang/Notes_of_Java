![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=u48f9f116&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# HttpMessageConverter
HttpMessageConverter是Spring MVC中非常重要的一个接口。翻译为：HTTP消息转换器。该接口下提供了很多实现类，不同的实现类有不同的转换方式。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711000445139-8bc9f74d-6ec3-4942-8063-5a130eac64eb.png#averageHue=%23e3e6be&clientId=uc1aa87e7-9dc5-4&from=paste&height=630&id=u739b94f0&originHeight=630&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=183370&status=done&style=shadow&taskId=ucf6e1b45-5eaa-4332-ab78-87e4fa043f1&title=&width=640)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=N1yjp&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 什么是HTTP消息
HTTP消息其实就是HTTP协议。HTTP协议包括请求协议和响应协议。
以下是一份HTTP POST请求协议：
```
POST /springmvc/user/login HTTP/1.1																												--请求行
Content-Type: application/x-www-form-urlencoded																						--请求头
Content-Length: 32
Host: www.example.com
User-Agent: Mozilla/5.0
Connection: Keep-Alive
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
                                                                                          --空白行
username=admin&password=1234																															--请求体
```
以下是一份HTTP GET请求协议：
```
GET /springmvc/user/del?id=1&name=zhangsan HTTP/1.1																				--请求行
Host: www.example.com																																			--请求头
User-Agent: Mozilla/5.0
Connection: Keep-Alive
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
```
以下是一份HTTP响应协议：
```
HTTP/1.1 200 OK																																					--状态行
Date: Thu, 01 Jul 2021 06:35:45 GMT																											--响应头
Content-Type: text/plain; charset=utf-8
Content-Length: 12
Connection: keep-alive
Server: Apache/2.4.43 (Win64) OpenSSL/1.1.1g
                                                                                        --空白行
<!DOCTYPE html>																																					--响应体
<html>
  <head>
    <title>hello</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=L2mmN&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 转换器转换的是什么
转换的是`HTTP协议`与`Java程序中的对象`之间的互相转换。请看下图：
![无标题.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711002146899-deaef9c8-a3b7-425e-97b1-6ada5477c674.png#averageHue=%23fbfbfb&clientId=uc1aa87e7-9dc5-4&from=paste&height=581&id=u0dac3ab1&originHeight=581&originWidth=692&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41637&status=done&style=shadow&taskId=ud9c7db5e-0b6a-4d34-a2cd-af50a96810e&title=&width=692)
上图是我们之前经常写的代码。请求体中的数据是如何转换成user对象的，底层实际上使用了`HttpMessageConverter`接口的其中一个实现类`FormHttpMessageConverter`。
通过上图可以看出`FormHttpMessageConverter`是负责将`请求协议`转换为`Java对象`的。

再看下图：
![无标题.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711003362257-f736f7c8-4d55-4e3f-b8f8-cfbab97c21f4.png#averageHue=%23fbfbfb&clientId=uc1aa87e7-9dc5-4&from=paste&height=460&id=u20f64128&originHeight=460&originWidth=1540&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49114&status=done&style=shadow&taskId=uec4ad039-ac13-4c49-b826-2bec563b880&title=&width=1540)
上图的代码也是之前我们经常写的，Controller返回值看做逻辑视图名称，视图解析器将其转换成物理视图名称，生成视图对象，`StringHttpMessageConverter`负责将视图对象中的HTML字符串写入到HTTP协议的响应体中。最终完成响应。
通过上图可以看出`StringHttpMessageConverter`是负责将`Java对象`转换为`响应协议`的。


![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=C3psC&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

通过以上内容的学习，大家应该能够了解到`HttpMessageConverter`接口是用来做什么的了：
![无标题.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711003929875-072161b4-af27-4855-9980-5d8ba186730b.png#averageHue=%23ececeb&clientId=uc1aa87e7-9dc5-4&from=paste&height=109&id=ud67955dd&originHeight=109&originWidth=1210&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7784&status=done&style=shadow&taskId=u9f2251ef-a356-46d9-9dfe-52f86b79c11&title=&width=1210)
如上图所示：HttpMessageConverter接口的可以将请求协议转换成Java对象，也可以把Java对象转换为响应协议。
**HttpMessageConverter是接口，SpringMVC帮我们提供了非常多而丰富的实现类。每个实现类都有自己不同的转换风格。**
**对于我们程序员来说，Spring MVC已经帮助我们写好了，我们只需要在不同的业务场景下，选择合适的HTTP消息转换器即可。**
**怎么选择呢？当然是通过SpringMVC为我们提供的注解，我们通过使用不同的注解来启用不同的消息转换器。**

在HTTP消息转换器这一小节，我们重点要掌握的是两个注解两个类：

- @ResponseBody
- @RequestBody
- ResponseEntity
- RequestEntity

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=WATjC&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# Spring MVC中的AJAX请求
SpringMVC+Vue3+Thymeleaf+Axios发送一个简单的AJAX请求。

引入Vue和Axios的js文件：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711010958303-5c6378c5-1d6e-4736-a2af-02ea04aa2f4c.png#averageHue=%23f1f3f8&clientId=u2fdcbe87-8e74-4&from=paste&height=362&id=u5de5c34d&originHeight=362&originWidth=344&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23944&status=done&style=none&taskId=ud08359f2-933d-44bd-b95a-fb94356150d&title=&width=344)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.powernode.springmvc.controller"/>

    <!--视图解析器-->
    <bean id="thymeleafViewResolver" class="org.thymeleaf.spring6.view.ThymeleafViewResolver">
        <property name="characterEncoding" value="UTF-8"/>
        <property name="order" value="1"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring6.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring6.templateresolver.SpringResourceTemplateResolver">
                        <property name="prefix" value="/WEB-INF/thymeleaf/"/>
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML"/>
                        <property name="characterEncoding" value="UTF-8"/>
                    </bean>
                </property>
            </bean>
        </property>
    </bean>

    <!--视图控制器映射-->
    <mvc:view-controller path="/" view-name="index"/>

    <!--开启注解驱动-->
    <mvc:annotation-driven/>

    <!--静态资源处理-->
    <mvc:default-servlet-handler/>

</beans>
```
重点是静态资源处理、开启注解驱动、视图控制器映射等相关配置。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=cSz5d&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

Vue3+Thymeleaf+Axios发送AJAX请求:
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
    <script th:src="@{/static/js/vue3.4.21.js}"></script>
    <script th:src="@{/static/js/axios.min.js}"></script>
</head>
<body>
<h1>首页</h1>
<hr>

<div id="app">
    <h1>{{message}}</h1>
    <button @click="getMessage">获取消息</button>
</div>

<script th:inline="javascript">
    Vue.createApp({
        data(){
            return {
                message : "这里的信息将被刷新"
            }
        },
        methods:{
            async getMessage(){
                try {
                    const response = await axios.get([[@{/}]] + 'hello')
                    this.message = response.data
                }catch (e) {
                    console.error(e)
                }
            }
        }
    }).mount("#app")
</script>

</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=K47Pu&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

**重点来了，Controller怎么写呢，之前我们都是传统的请求，Controller返回一个**`**逻辑视图名**`**，然后交给**`**视图解析器**`**解析。最后跳转页面。而AJAX请求是不需要跳转页面的，因为AJAX是页面局部刷新，以前我们在Servlet中使用**`**response.getWriter().print("message")**`**的方式响应。在Spring MVC中怎么办呢？当然，我们在Spring MVC中也可以使用Servlet原生API来完成这个功能，代码如下：**
```java
package com.powernode.springmvc.controller;

import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import java.io.IOException;

@Controller
public class HelloController {

    @RequestMapping(value = "/hello")
    public String hello(HttpServletResponse response) throws IOException {
        response.getWriter().print("hello");
        return null;
    }
}

```
或者这样也行：不需要有返回值
```java
package com.powernode.springmvc.controller;

import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import java.io.IOException;

@Controller
public class HelloController {

    @RequestMapping(value = "/hello")
    public void hello(HttpServletResponse response) throws IOException {
        response.getWriter().print("hello");
    }
}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=uUR2K&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

启动服务器测试：[http://localhost:8080/springmvc/](http://localhost:8080/springmvc/)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711011917028-242026ab-86de-409b-8a91-13f3cbb1b142.png#averageHue=%23f1f1f0&clientId=u2fdcbe87-8e74-4&from=paste&height=262&id=u70108787&originHeight=262&originWidth=441&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15690&status=done&style=shadow&taskId=u2798242b-17d1-4247-b9fa-94d305e6283&title=&width=441)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711011931023-727ffe37-387a-4b75-b594-9fec8b7d7944.png#averageHue=%23f8f7f7&clientId=u2fdcbe87-8e74-4&from=paste&height=269&id=udf289c55&originHeight=269&originWidth=388&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9908&status=done&style=shadow&taskId=u2b3434d6-444c-40b6-b4c4-0bc2df2060a&title=&width=388)
**注意：如果采用这种方式响应，则和 springmvc.xml 文件中配置的视图解析器没有关系，不走视图解析器了。**

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=o1XrB&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

难道我们以后AJAX请求都要使用原生Servlet API吗？

- 不需要，我们可以使用SpringMVC中提供的HttpMessageConverter消息转换器。

我们要向前端响应一个字符串"hello"，这个"hello"就是响应协议中的响应体。
我们可以使用 @ResponseBody 注解来启用对应的消息转换器。而这种消息转换器只负责将Controller返回的信息以响应体的形式写入响应协议。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=o5khW&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# @ResponseBody
## StringHttpMessageConverter
上面的AJAX案例，Controller的代码可以修改为：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @RequestMapping(value = "/hello")
    @ResponseBody
    public String hello(){
        // 由于你使用了 @ResponseBody 注解
        // 以下的return语句返回的字符串则不再是“逻辑视图名”了
        // 而是作为响应协议的响应体进行响应。
        return "hello";
    }
}
```
最核心需要理解的位置是：return "hello";
这里的"hello"不是逻辑视图名了，而是作为响应体的内容进行响应。直接输出到浏览器客户端。
以上程序中使用的消息转换器是：**StringHttpMessageConverter**，为什么会启用这个消息转换器呢？因为你添加`@ResponseBody`这个注解。

通常AJAX请求需要服务器给返回一段JSON格式的字符串，可以返回JSON格式的字符串吗？当然可以，代码如下：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @RequestMapping(value = "/hello")
    @ResponseBody
    public String hello(){
        return "{\"username\":\"zhangsan\",\"password\":\"1234\"}";
    }
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=PgoJn&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711013196948-31c55e31-5868-40e9-b75c-f84810ef3056.png#averageHue=%23f6f5f4&clientId=u2fdcbe87-8e74-4&from=paste&height=259&id=uf76c1469&originHeight=259&originWidth=821&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18587&status=done&style=shadow&taskId=ue9eade83-dcb9-47f8-94b8-a06c4d7e62b&title=&width=821)
这是完全可以的，此时底层使用的消息转换器还是：**StringHttpMessageConverter**

那如果在程序中是一个POJO对象，怎么将POJO对象以JSON格式的字符串响应给浏览器呢？两种方式：

- 第一种方式：自己写代码将POJO对象转换成JSON格式的字符串，用上面的方式直接return即可。
- 第二种方式：启用`MappingJackson2HttpMessageConverter`消息转换器。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=yHUOt&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## MappingJackson2HttpMessageConverter
启用MappingJackson2HttpMessageConverter消息转换器的步骤如下：

第一步：引入jackson依赖，可以将java对象转换为json格式字符串
```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.17.0</version>
</dependency>
```

第二步：开启注解驱动
这一步非常关键，开启注解驱动后，在HandlerAdapter中会自动装配一个消息转换器：MappingJackson2HttpMessageConverter
```xml
<mvc:annotation-driven/>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=AZC18&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

第三步：准备一个POJO
```java
package com.powernode.springmvc.bean;

public class User {
    private String username;
    private String password;

    public User() {
    }

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=IfkX3&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

第四步：控制器方法使用 @ResponseBody 注解标注(非常重要），控制器方法返回这个POJO对象
```java
package com.powernode.springmvc.controller;

import com.powernode.springmvc.bean.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @RequestMapping(value = "/hello")
    @ResponseBody
    public User hello(){
        User user = new User("zhangsan", "22222");
        return user;
    }
}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=yfu96&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711014082618-8a46beab-d498-4d67-abad-662e07d5871f.png#averageHue=%23f6f6f5&clientId=u2fdcbe87-8e74-4&from=paste&height=284&id=u0c94604d&originHeight=284&originWidth=843&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18664&status=done&style=shadow&taskId=udce9980a-01af-435b-bfea-254d65dea11&title=&width=843)

以上代码底层启用的就是 MappingJackson2HttpMessageConverter 消息转换器。
他的功能很强大，可以将POJO对象转换成JSON格式的字符串，响应给前端。
其实这个消息转换器`MappingJackson2HttpMessageConverter`本质上只是比`StringHttpMessageConverter`多了一个json字符串的转换，其他的还是一样。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=ArFCN&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# @RestController
因为我们现代的开发方式都是基于AJAX方式的，因此 @ResponseBody 注解非常重要，很常用。
为了方便，Spring MVC中提供了一个注解 @RestController。这一个注解代表了：@Controller + @ResponseBody。
@RestController 标注在类上即可。被它标注的Controller中所有的方法上都会自动标注 @ResponseBody

```java
package com.powernode.springmvc.controller;

import com.powernode.springmvc.bean.User;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @RequestMapping(value = "/hello")
    public User hello(){
        User user = new User("zhangsan", "22222");
        return user;
    }
}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=eZQl6&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711014419291-3b5e131c-f81f-4054-9a03-295323dee8d4.png#averageHue=%23f6f5f4&clientId=u2fdcbe87-8e74-4&from=paste&height=266&id=u52ca289a&originHeight=266&originWidth=827&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18568&status=done&style=shadow&taskId=u0282b26a-291c-42d5-84fc-fdf0a178a7e&title=&width=827)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=G0kQ6&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# @RequestBody
## FormHttpMessageConverter
这个注解的作用是直接将请求体传递给Java程序，在Java程序中可以直接使用一个String类型的变量接收这个请求体的内容。

在没有使用这个注解的时候：
```java
@RequestMapping("/save")
public String save(User user){
    // 执行保存的业务逻辑
    userDao.save(user);
    // 保存成功跳转到成功页面
    return "success";
}
```
当请求体提交的数据是：
```
username=zhangsan&password=1234&email=zhangsan@powernode.com
```
那么Spring MVC会自动使用 `FormHttpMessageConverter`消息转换器，将请求体转换成user对象。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=jjdVc&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

当使用这个注解的时候：**这个注解只能出现在方法的参数上。**
```java
@RequestMapping("/save")
public String save(@RequestBody String requestBodyStr){
    System.out.println("请求体：" + requestBodyStr);
    return "success";
}
```
Spring MVC仍然会使用 `FormHttpMessageConverter`消息转换器，将请求体直接以字符串形式传递给 requestBodyStr 变量。
测试输出结果：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711022270055-a1599817-6c63-4d06-bfe6-52c10bcdf3ef.png#averageHue=%23f5e3de&clientId=u2fdcbe87-8e74-4&from=paste&height=81&id=u5823cf53&originHeight=81&originWidth=697&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20811&status=done&style=shadow&taskId=u5406b47e-fcaf-4cf1-b0ac-bc21d9dff70&title=&width=697)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=kLqz9&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## MappingJackson2HttpMessageConverter
另外，如果在请求体中提交的是一个JSON格式的字符串，这个JSON字符串传递给Spring MVC之后，能不能将JSON字符串转换成POJO对象呢？答案是可以的。
此时必须使用@RequestBody 注解来完成 。并且底层使用的消息转换器是：`MappingJackson2HttpMessageConverter`。实现步骤如下：

- 第一步：引入jackson依赖
- 第二步：开启注解驱动
- 第三步：创建POJO类，将POJO类作为控制器方法的参数，并使用 @RequestBody 注解标注该参数
```java
@RequestMapping("/send")
@ResponseBody
public String send(@RequestBody User user){
    System.out.println(user);
    System.out.println(user.getUsername());
    System.out.println(user.getPassword());
    return "success";
}
```

- 第四步：在请求体中提交json格式的数据
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
    <script th:src="@{/static/js/vue3.4.21.js}"></script>
    <script th:src="@{/static/js/axios.min.js}"></script>
</head>
<body>

<div id="app">
    <button @click="sendJSON">通过POST请求发送JSON给服务器</button>
    <h1>{{message}}</h1>
</div>

<script>
    let jsonObj = {"username":"zhangsan", "password":"1234"}

    Vue.createApp({
        data(){
            return {
                message:""
            }
        },
        methods: {
            async sendJSON(){
                console.log("sendjson")
                try{
                    const res = await axios.post('/springmvc/send', JSON.stringify(jsonObj), {
                        headers : {
                            "Content-Type" : "application/json"
                        }
                    })
                    this.message = res.data
                }catch(e){
                    console.error(e)
                }
            }
        }
    }).mount("#app")
</script>

</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=aXF7W&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

测试结果：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711024282143-bde87ec5-476e-470e-a9fa-94a0f2858938.png#averageHue=%23f1efee&clientId=u2fdcbe87-8e74-4&from=paste&height=111&id=uc1d6aa8a&originHeight=111&originWidth=294&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5364&status=done&style=shadow&taskId=ub786438d-1897-40a6-99f5-b0bb8cb1e92&title=&width=294)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711024299450-33c514e9-a7b1-4010-8d9c-8bd7824a9dd6.png#averageHue=%23f9eeeb&clientId=u2fdcbe87-8e74-4&from=paste&height=143&id=u14a6426a&originHeight=143&originWidth=459&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19019&status=done&style=shadow&taskId=ua3a29b07-da66-41f3-9af1-ea56cfbc128&title=&width=459)


![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=ioFuq&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# RequestEntity
RequestEntity不是一个注解，是一个普通的类。这个类的实例封装了整个请求协议：包括请求行、请求头、请求体所有信息。
出现在控制器方法的参数上：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
    <script th:src="@{/static/js/vue3.4.21.js}"></script>
    <script th:src="@{/static/js/axios.min.js}"></script>
</head>
<body>

<div id="app">
    <button @click="sendJSON">通过POST请求发送JSON给服务器</button>
    <h1>{{message}}</h1>
</div>

<script>
    let jsonObj = {"username":"zhangsan", "password":"1234"}

    Vue.createApp({
        data(){
            return {
                message:""
            }
        },
        methods: {
            async sendJSON(){
                console.log("sendjson")
                try{
                    const res = await axios.post('/springmvc/send', JSON.stringify(jsonObj), {
                        headers : {
                            "Content-Type" : "application/json"
                        }
                    })
                    this.message = res.data
                }catch(e){
                    console.error(e)
                }
            }
        }
    }).mount("#app")
</script>

</body>
</html>
```
```java
@RequestMapping("/send")
@ResponseBody
public String send(RequestEntity<User> requestEntity){
    System.out.println("请求方式：" + requestEntity.getMethod());
    System.out.println("请求URL：" + requestEntity.getUrl());
    HttpHeaders headers = requestEntity.getHeaders();
    System.out.println("请求的内容类型：" + headers.getContentType());
    System.out.println("请求头：" + headers);

    User user = requestEntity.getBody();
    System.out.println(user);
    System.out.println(user.getUsername());
    System.out.println(user.getPassword());
    return "success";
}
```
测试结果：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711032010156-cb98e4a9-5238-4dd6-ac1a-81dd6198a47d.png#averageHue=%23f9f2f0&clientId=u84c24dc6-b425-4&from=paste&height=253&id=u780ddfcd&originHeight=253&originWidth=615&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36432&status=done&style=shadow&taskId=u179ddeb5-a404-4236-8356-c24a241d08f&title=&width=615)
在实际的开发中，如果你需要获取更详细的请求协议中的信息。可以使用`RequestEntity`

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Kbp67&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# ResponseEntity
ResponseEntity不是注解，是一个类。用该类的实例可以封装响应协议，包括：状态行、响应头、响应体。也就是说：如果你想定制属于自己的响应协议，可以使用该类。
假如我要完成这样一个需求：前端提交一个id，后端根据id进行查询，如果返回null，请在前端显示404错误。如果返回不是null，则输出返回的user。
```java
@Controller
public class UserController {
     
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        User user = userService.getUserById(id);
        if (user == null) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).body(null);
        } else {
            return ResponseEntity.ok(user);
        }
    }
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=bZQpt&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

测试：当用户不存在时
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711032765280-343794d6-b262-460b-8c03-e14bd8946850.png#averageHue=%23fefefd&clientId=u84c24dc6-b425-4&from=paste&height=558&id=u565ba7e8&originHeight=558&originWidth=1159&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22933&status=done&style=shadow&taskId=u42f8faab-481d-43af-aa7d-1cc81ae26d8&title=&width=1159)

测试：当用户存在时
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711032830325-866fe36b-cc47-4493-b9bb-8ebd34c7a86c.png#averageHue=%23d6b482&clientId=u84c24dc6-b425-4&from=paste&height=159&id=u262cf82e&originHeight=159&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14457&status=done&style=shadow&taskId=u41ee96bd-de5c-4a78-bbd2-f90396db356&title=&width=515)
