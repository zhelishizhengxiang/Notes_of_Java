# Thymeleaf视图
我们在学习前面内容的时候，采用的都是Thymeleaf视图。我们再来测试一下，看看底层创建的视图对象是不是`ThymeleafView`

springmvc.xml配置内容如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

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
</beans>
```
Controller代码如下：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class IndexController {
    @RequestMapping("/index")
    public String toIndex(){
        return "index";
    }
}

```
视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>index page</title>
</head>
<body>
<h1>index page</h1>
</body>
</html>
```

添加断点：在DispatcherServlet的doDispatch方法的下图位置添加断点  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710835859057-703d8177-8e9c-4a42-9f8d-e36d0bfb1e42.png#averageHue=%23fdfaf8&clientId=u6bedb242-223f-4&from=paste&height=285&id=uc3fe801e&originHeight=285&originWidth=1004&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30503&status=done&style=shadow&taskId=ude602775-dc45-492d-87c4-4dbffb33b0e&title=&width=1004)

启动Tomcat，在浏览器地址栏上发送请求：http://localhost:8080/springmvc/index  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710835931836-b1a27108-f01b-49ad-a5f7-308ad0cf7f8b.png#averageHue=%23f4e5c6&clientId=u6bedb242-223f-4&from=paste&height=229&id=u700a1239&originHeight=229&originWidth=687&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24231&status=done&style=shadow&taskId=u678bf0c4-f009-4d09-b4f0-f8e85aa0634&title=&width=687)

程序走到以上位置，这行代码是调用对应的Controller，并且Controller最终会返回ModelAndView对象：mv

按照我们之前所讲，返回mv之后，接下来就是视图处理与渲染，接着往下走，走到下图这一行：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710836061330-46ee32ce-5549-4758-85f3-0dd8c0b20079.png#averageHue=%23d5b48c&clientId=u6bedb242-223f-4&from=paste&height=137&id=u28cd67a9&originHeight=137&originWidth=783&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23569&status=done&style=shadow&taskId=ud989316d-b78c-4428-9be1-5122d812f8b&title=&width=783)
* 这个方法的作用是处理分发结果，就是在这个方法当中进行了视图的处理与渲染，进入该方法：

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710836134539-34cc0424-ea05-4045-810d-56b063b59fb4.png#averageHue=%23fcfaf8&clientId=u6bedb242-223f-4&from=paste&height=479&id=u73ef7405&originHeight=479&originWidth=816&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90720&status=done&style=shadow&taskId=ufc4f601d-189d-4885-81b4-705d95e7db6&title=&width=816)
* 进去之后走到上图位置：这个方法就是用来渲染页面的方法，再进入该方法：

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710836196992-3d3ef841-db8b-4642-aa9a-fa2ffef5ef0e.png#averageHue=%23faf6f4&clientId=u6bedb242-223f-4&from=paste&height=264&id=u048d73aa&originHeight=264&originWidth=580&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52949&status=done&style=shadow&taskId=u56330a98-d26c-4dd5-b182-21f4d11dd0d&title=&width=580)
走到上图位置就可以看到底层创建的是ThymeleafView对象。


# JSP视图（了解）
我们再来跟一下源码，看看JSP视图底层创建的是不是InternalResourceView对象。
我们前面说过 InternalResourceView是SpringMVC框架内置的，翻译为内部资源视图，SpringMVC把JSP看做是内部资源。可见JSP在之前的技术栈中有很高的地位。
不过，当下流行的开发中JSP使用较少，这里不再详细讲解。只是测试一下。
springmvc.xml配置如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.powernode.springmvc.controller"/>

    <!--视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```
Controller代码如下：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class IndexController {
    @RequestMapping("/index")
    public String toIndex(){
        return "index";
    }
}
```
视图页面：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>index jsp</title>
  </head>
  <body>
    <h1>index jsp!</h1>
  </body>
</html>
```

启动web容器，添加断点跟踪：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710836651520-2ea9a9ba-0a71-4f3e-977c-4bce0ddfdcf8.png#averageHue=%23fbf6f5&clientId=u6bedb242-223f-4&from=paste&height=265&id=ua9f75b72&originHeight=265&originWidth=481&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42624&status=done&style=shadow&taskId=u080e28c2-8c70-4da4-a5eb-a62d15d6818&title=&width=481)
通过测试得知：对于JSP视图来说，底层创建的视图对象是InternalResourceView。

