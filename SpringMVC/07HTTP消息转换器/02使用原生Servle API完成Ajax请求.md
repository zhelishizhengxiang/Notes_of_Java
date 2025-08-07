
**SpringMVC+Vue3+Thymeleaf+Axios发送一个简单的AJAX请求**。

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


* **重点来了，Controller怎么写呢，之前我们都是传统的请求，Controller返回一个**`**逻辑视图名**`**，然后交给**`**视图解析器**`**解析。最后跳转页面。
* **而AJAX请求是不需要跳转页面的，因为AJAX是页面局部刷新**，以前我们在Servlet中使用**`**response.getWriter().print("message")**`**的方式响应。在Spring MVC中怎么办呢？当然，我们在Spring MVC中也可以使用Servlet原生API来完成这个功能，代码如下：**
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



启动服务器测试：[http://localhost:8080/springmvc/](http://localhost:8080/springmvc/)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711011917028-242026ab-86de-409b-8a91-13f3cbb1b142.png#averageHue=%23f1f1f0&clientId=u2fdcbe87-8e74-4&from=paste&height=262&id=u70108787&originHeight=262&originWidth=441&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15690&status=done&style=shadow&taskId=u2798242b-17d1-4247-b9fa-94d305e6283&title=&width=441)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711011931023-727ffe37-387a-4b75-b594-9fec8b7d7944.png#averageHue=%23f8f7f7&clientId=u2fdcbe87-8e74-4&from=paste&height=269&id=udf289c55&originHeight=269&originWidth=388&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9908&status=done&style=shadow&taskId=u2b3434d6-444c-40b6-b4c4-0bc2df2060a&title=&width=388)
**注意：如果采用这种方式响应，则和 springmvc.xml 文件中配置的视图解析器没有关系，不走视图解析器了。**



难道我们以后AJAX请求都要使用原生Servlet API吗？

- 不需要，我们可以使用SpringMVC中提供的HttpMessageConverter消息转换器。

我们要向前端响应一个字符串"hello"，这个"hello"就是响应协议中的响应体。
我们可以使用 @ResponseBody 注解来启用对应的消息转换器。而这种消息转换器只负责将Controller返回的信息以响应体的形式写入响应协议。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=o5khW&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400