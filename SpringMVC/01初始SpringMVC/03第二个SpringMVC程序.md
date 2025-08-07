## 1.创建Maven模块

1. pom.xml文件中添加依赖
2. springmvc依赖
3. logback依赖
4. servlet依赖（scope为provided）
5. thymeleaf与spring6整合依赖
6. 打包方式war


## 2.添加web支持
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710315550290-18c819de-15fb-4653-a242-8ac1c8d1255d.png#averageHue=%23eef1f8&clientId=u0dd2e7db-835e-4&from=paste&height=198&id=u1bc1a1cd&originHeight=198&originWidth=219&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9851&status=done&style=shadow&taskId=u9ae36ed0-8799-403a-843d-6ea0c0f28a6&title=&width=219)
webapp目录没有小蓝点怎么办？添加web支持    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710315591600-e1e8f89d-9731-40ee-b601-04c8b2923258.png#averageHue=%23b2cec4&clientId=u0dd2e7db-835e-4&from=paste&height=373&id=u6dc66d24&originHeight=373&originWidth=369&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33616&status=done&style=shadow&taskId=u8345e6f1-5eb2-4184-bb3b-37bceb8ac17&title=&width=369)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710315690201-7d425088-0775-4e64-97a3-e33c09374add.png#averageHue=%23f4f5f9&clientId=u0dd2e7db-835e-4&from=paste&height=590&id=u16041e99&originHeight=590&originWidth=1359&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44597&status=done&style=shadow&taskId=u685dcb7d-f2e7-4a7b-a636-3807141b073&title=&width=1359)


## 3.配置web.xml文件
**重点：SpringMVC配置文件的名字和路径是可以手动设置的，如下：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--手动设置springmvc配置文件的路径及名字-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--为了提高用户的第一次访问效率，建议在web服务器启动时初始化前端控制器-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```
* **在设置DispatcherServlet的时候，通过\<init-param>来设置SpringMVC配置文件的路径和名字。这些配置是在DispatcherServlet的init方法执行时设置的。其中\<param-name>指定为contextConfigLocation；\<param-value>指定为classpath:springmvc.xml，表示在类的根路径下有一个文件是springmvc.xml，该文件就是SpringMVC的配置文件。**
* **\<load-on-startup>1</load-on-startup>建议加上，这样可以提高用户第一次访问的效率。表示在web服务器启动时就创建并初始化DispatcherServlet。** 该参数默认值为-1，表示第一次访问才创建该Servlet对象。


## 4.编写IndexController
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: IndexController
 * Description:
 * Datetime: 2024/3/13 15:47
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class IndexController {
    @RequestMapping("/")
    public String toIndex(){
        return "index";
    }
}
```
**表示请求路径如果是：[http://localhost:8080/springmvc/](http://localhost:8080/springmvc/) ，则进入 /WEB-INF/templates/index.html 页面。**

**这就是项目的首页效果！！！！！**


## 5.在resources目录下配置springmvc.xml文件
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710316235461-25d719f5-5b8f-4600-922a-8568d9cd63dc.png#averageHue=%23f0f3f8&clientId=u0dd2e7db-835e-4&from=paste&height=315&id=u6e0a9fe3&originHeight=315&originWidth=366&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21866&status=done&style=shadow&taskId=ubf2c7ec6-0ca1-437f-9ab5-941d3f16c50&title=&width=366)

配置内容和之前一样，一个是视图解析器，一个是组件扫描。


## 6.提供视图
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710316353838-aac1cd57-12e3-47e4-8b73-2ea2a07a0954.png#averageHue=%23eef0f4&clientId=u0dd2e7db-835e-4&from=paste&height=128&id=u9a159361&originHeight=128&originWidth=289&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7935&status=done&style=shadow&taskId=uaf3f4b6a-fb23-4ec5-8294-00978351f60&title=&width=289)
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

## 7.测试
部署到web服务器，启动web服务器，打开浏览器，在地址栏上输入：[http://localhost:8080/springmvc/](http://localhost:8080/springmvc/)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710317491301-4104920d-3537-40d1-b950-2ad1f3398a2d.png#averageHue=%23f7f7f6&clientId=u0dd2e7db-835e-4&from=paste&height=170&id=uc8b7b484&originHeight=170&originWidth=421&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8615&status=done&style=shadow&taskId=u9896bf63-92fb-4dd5-9584-0cbffeb8277&title=&width=421)
