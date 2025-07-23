![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=u48f9f116&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# RESTFul编程风格
## RESTFul是什么
RESTFul是`WEB服务接口`的一种设计风格。
RESTFul定义了一组约束条件和规范，可以让`WEB服务接口`更加简洁、易于理解、易于扩展、安全可靠。

RESTFul对一个`WEB服务接口`都规定了哪些东西？

- 对请求的URL格式有约束和规范
- 对HTTP的请求方式有约束和规范
- 对请求和响应的数据格式有约束和规范
- 对HTTP状态码有约束和规范
- 等 ......

REST对请求方式的约束是这样的：

- 查询必须发送GET请求
- 新增必须发送POST请求
- 修改必须发送PUT请求
- 删除必须发送DELETE请求

REST对URL的约束是这样的：

- 传统的URL：get请求，/springmvc/getUserById?id=1
- REST风格的URL：get请求，/springmvc/user/1

- 传统的URL：get请求，/springmvc/deleteUserById?id=1
- REST风格的URL：delete请求, /springmvc/user/1


![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=soshf&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

RESTFul对URL的约束和规范的核心是：**通过采用**`**不同的请求方式**`**+ **`**URL**`**来确定WEB服务中的资源。**

**RESTful 的英文全称是 Representational State Transfer（表述性状态转移）。简称REST。**
表述性（Representational）是：URI + 请求方式。
状态（State）是：服务器端的数据。
转移（Transfer）是：变化。
表述性状态转移是指：通过 URI + 请求方式 来控制服务器端数据的变化。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=TV7Oz&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## RESTFul风格与传统方式对比
传统的 URL 与 RESTful URL 的区别是传统的 URL 是基于方法名进行资源访问和操作，而 RESTful URL 是基于资源的结构和状态进行操作的。下面是一张表格，展示两者之间的具体区别：

| **传统的 URL** | **RESTful URL** |
| --- | --- |
| GET /getUserById?id=1 | GET /user/1 |
| GET /getAllUser | GET /user |
| POST /addUser | POST /user |
| POST /modifyUser | PUT /user |
| GET /deleteUserById?id=1 | DELETE /user/1 |

从上表中我们可以看出，传统的URL是基于动作的，而 RESTful URL 是基于资源和状态的，因此 RESTful URL 更加清晰和易于理解，这也是 REST 架构风格被广泛使用的主要原因之一。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Igybe&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## RESTFul方式演示查询
RESTFul规范中规定，如果要查询数据，需要发送GET请求。
### 根据id查询(GET /api/user/1)
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

    <!--启用注解-->
    <mvc:annotation-driven/>

    <!--视图控制器映射-->
    <mvc:view-controller path="/" view-name="index"/>
</beans>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=su0jN&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

首页index.html
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
<h1>index page</h1>
<hr>
<!--根据id查询：GET /api/user/1 -->
<a th:href="@{/api/user/1}">根据id查询用户信息</a><br>

</body>
</html>
```

控制器Controller：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class UserController {

    @RequestMapping(value = "/api/user/{id}", method = RequestMethod.GET)
    public String getById(@PathVariable("id") Integer id){
        System.out.println("根据用户id查询用户信息，用户id是" + id);
        return "ok";
    }

}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=P2TkR&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>ok</title>
</head>
<body>
<h1>ok</h1>
</body>
</html>
```

启动服务器，测试：http://localhost:8080/springmvc
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710945843656-59d204d5-daa9-4e89-a977-686bc2642a33.png#averageHue=%23f7f7f6&clientId=u056440f2-a263-4&from=paste&height=171&id=u7f187897&originHeight=171&originWidth=474&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10133&status=done&style=shadow&taskId=u64952ae8-6422-40b0-a6f3-d542285f25a&title=&width=474)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710945859052-7f4aab9c-2e94-4926-b8ec-9cd01483e51e.png#averageHue=%23faf9f8&clientId=u056440f2-a263-4&from=paste&height=115&id=u995e3a08&originHeight=115&originWidth=433&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6421&status=done&style=shadow&taskId=u03a1b58c-a50f-48a8-a08e-1880526b7db&title=&width=433)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710945874351-0ef0c930-f425-449c-9649-60de8b88958e.png#averageHue=%23f4f2f0&clientId=u056440f2-a263-4&from=paste&height=109&id=uf838ec9b&originHeight=109&originWidth=380&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10515&status=done&style=shadow&taskId=ub5effe8a-de20-4214-b119-bf698b926fc&title=&width=380)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=gRvXu&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

### 查询所有(GET /api/user)
```html
<!--查询所有-->
<a th:href="@{/api/user}">查询所有</a><br>
```
```java
@RequestMapping(value = "/api/user", method = RequestMethod.GET)
public String getAll(){
    System.out.println("查询所有用户信息");
    return "ok";
}
```
启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946048811-bacdfc38-344d-4468-9a85-fbc0ef8ecb28.png#averageHue=%23f7f6f6&clientId=u056440f2-a263-4&from=paste&height=197&id=uaf007617&originHeight=197&originWidth=389&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10934&status=done&style=shadow&taskId=ub52e23f4-f31b-46d7-b4cc-08c27b1e13f&title=&width=389)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946060913-d1555b77-229d-4993-8363-ba8afaa78e6a.png#averageHue=%23faf9f8&clientId=u056440f2-a263-4&from=paste&height=111&id=u6c70726f&originHeight=111&originWidth=427&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6303&status=done&style=shadow&taskId=ua4bf99ce-60e0-4749-aad2-c2368e77557&title=&width=427)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946074461-7b8a5427-3e95-495b-80c4-99b7024458a4.png#averageHue=%23f5f3f0&clientId=u056440f2-a263-4&from=paste&height=107&id=u4c99cb0f&originHeight=107&originWidth=343&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10321&status=done&style=shadow&taskId=u1cb6615a-d617-4970-8a94-2d92a0d07fc&title=&width=343)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=MYCFR&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## RESTFul方式演示增加(POST /api/user)
RESTFul规范中规定，如果要进行保存操作，需要发送POST请求。
```html
<!--保存用户-->
<form th:action="@{/api/user}" method="post">
    <input type="submit" th:value="保存">
</form>
```

```java
@RequestMapping(value = "/api/user", method = RequestMethod.POST)
public String save(){
    System.out.println("保存用户信息");
    return "ok";
}
```

启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946440909-1140c31e-f921-42fe-bbab-5a9c4409f388.png#averageHue=%23f7f6f6&clientId=u056440f2-a263-4&from=paste&height=212&id=u9585b4cb&originHeight=212&originWidth=395&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11521&status=done&style=shadow&taskId=u0e442a6e-4100-4ac5-b294-d78f1c63333&title=&width=395)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946457841-33c623d2-75a6-4486-bfb0-472e9f3ae72e.png#averageHue=%23fafaf9&clientId=u056440f2-a263-4&from=paste&height=123&id=u724fe648&originHeight=123&originWidth=417&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6348&status=done&style=shadow&taskId=uc8c798af-dbfd-4d55-b487-7ff2a7b161d&title=&width=417)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946468943-5ecc05c9-cc83-47b7-966b-8fe0df99a3f5.png#averageHue=%23f6f3f1&clientId=u056440f2-a263-4&from=paste&height=108&id=ude85b2fc&originHeight=108&originWidth=247&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6746&status=done&style=shadow&taskId=u858b1229-a0ce-4759-8d5a-aa35ff1fc3d&title=&width=247)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=zCBDr&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## RESTFul方式演示修改
RESTFul规范中规定，如果要进行保存操作，需要发送PUT请求。
**如何发送PUT请求？**
**第一步：首先你必须是一个POST请求。**
**第二步：在发送POST请求的时候，提交这样的数据：**`**_method=PUT**`
**第三步：在web.xml文件配置SpringMVC提供的过滤器：HiddenHttpMethodFilter**

实践一下：
```html
<!--修改用户-->
<hr>
<form th:action="@{/api/user}" method="post">
    <!--隐藏域的方式提交 _method=put -->
    <input type="hidden" name="_method" value="put">
    用户名：<input type="text" name="username"><br>
    <input type="submit" th:value="修改">
</form>
```
```xml
<!--隐藏的HTTP请求方式过滤器-->
<filter>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
```java
@RequestMapping(value = "/api/user", method = RequestMethod.PUT)
public String update(String username){
    System.out.println("修改用户信息，用户名：" + username);
    return "ok";
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=tIRUD&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

注意pom.xml文件中添加如下配置：
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.12.1</version>
            <configuration>
                <source>21</source>
                <target>21</target>
                <compilerArgs>
                    <arg>-parameters</arg>
                </compilerArgs>
            </configuration>
        </plugin>
    </plugins>
</build>
```
**一定要重新build一下：**
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710947331695-c0c43ede-7a5d-47ea-a758-7728c3fefe05.png#averageHue=%23e9e6e2&clientId=uac7d1915-c027-4&from=paste&height=206&id=ucbcfc408&originHeight=206&originWidth=681&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42356&status=done&style=shadow&taskId=u674322b0-ff01-461b-b4b1-cdbe34b21b7&title=&width=681)

测试结果：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946938192-71c77332-687b-4041-b855-80752a1cf020.png#averageHue=%23f7f4f4&clientId=u056440f2-a263-4&from=paste&height=306&id=u8d1f3f66&originHeight=306&originWidth=462&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14433&status=done&style=shadow&taskId=u21a3c84c-d9aa-41fe-8a08-2f0ad61addf&title=&width=462)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710947347325-5f5de0e1-d785-49d1-a391-d3e047ffdaa7.png#averageHue=%23fafaf9&clientId=uac7d1915-c027-4&from=paste&height=122&id=uf3516a2a&originHeight=122&originWidth=434&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6369&status=done&style=shadow&taskId=u37646c71-aaf5-4a77-84ac-698c9618205&title=&width=434)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710947365236-7f8a8846-85e1-436e-b875-9012d999b21e.png#averageHue=%23f3efeb&clientId=uac7d1915-c027-4&from=paste&height=115&id=ub7681e13&originHeight=115&originWidth=328&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13641&status=done&style=shadow&taskId=u398b802d-a642-4dbe-a32e-a424aa67ca5&title=&width=328)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=SrPmT&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

## HiddenHttpMethodFilter
HiddenHttpMethodFilter是Spring MVC框架提供的，专门用于RESTFul编程风格。
实现原理可以通过源码查看：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710981996209-5c66441b-0aa9-41a7-b71d-26b2ffb0f4f5.png#averageHue=%23fdfaf9&clientId=u1c5395e7-28c7-4&from=paste&height=587&id=u35d657b9&originHeight=587&originWidth=1291&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91768&status=done&style=shadow&taskId=u077ff919-ffea-49a1-9104-627c073a7e2&title=&width=1291)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710982160559-ffe20024-a10a-4aa2-b39e-44bebd0d3945.png#averageHue=%23e3d3b0&clientId=u1c5395e7-28c7-4&from=paste&height=134&id=u4b2501ea&originHeight=134&originWidth=699&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19516&status=done&style=shadow&taskId=ub9ce2e5e-f893-4b8c-8afc-1a59a08a49f&title=&width=699)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710982194265-720a0b49-aa95-475f-900b-7234280f5c9c.png#averageHue=%23fcfbfa&clientId=u1c5395e7-28c7-4&from=paste&height=93&id=u4e856ede&originHeight=93&originWidth=925&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14484&status=done&style=shadow&taskId=u88436ca4-4970-4d64-9b71-c8871fab154&title=&width=925)
通过源码可以看到，if语句中，首先判断是否为POST请求，如果是POST请求，调用`request.getParameter(this.methodParam)`。可以看到`this.methodParam`是`_method`，这样就要求我们在提交请求方式的时候必须采用这个格式：`_method=put`。获取到请求方式之后，调用了toUpperCase转换成大写了。因此前端页面中小写的put或者大写的PUT都是可以的。if语句中嵌套的if语句说的是，只有请求方式是 PUT,DELETE,PATCH的时候会创建HttpMethodRequestWrapper对象。而HttpMethodRequestWrapper对象的构造方法是这样的：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710984179119-96331e0b-ae39-45b0-bba1-b8db3ec7107f.png#averageHue=%23fefcfb&clientId=u1c5395e7-28c7-4&from=paste&height=395&id=udaf4f43c&originHeight=395&originWidth=905&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37994&status=done&style=shadow&taskId=u8c6216f0-6cfb-497c-802d-35c9446b8fe&title=&width=905)
这样method就从POST变成了：PUT/DELETE/PATCH。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=fLKLz&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

**重点注意事项：CharacterEncodingFilter和HiddenHttpMethodFilter的顺序**
细心的同学应该注意到了，在HiddenHttpMethodFilter源码中有这样一行代码：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710984264334-7df83331-ddbb-4ead-a58c-cb4dc6c19ef6.png#averageHue=%23f9f6f3&clientId=u1c5395e7-28c7-4&from=paste&height=46&id=ue805f02f&originHeight=46&originWidth=614&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11849&status=done&style=shadow&taskId=u135178c5-b2d9-4d70-9fa7-91cb3e8dfc9&title=&width=614)
大家是否还记得，字符编码过滤器执行之前不能调用 request.getParameter方法，如果提前调用了，乱码问题就无法解决了。因为request.setCharacterEncoding()方法的执行必须在所有request.getParameter()方法之前执行。因此这两个过滤器就有先后顺序的要求，在web.xml文件中，应该先配置CharacterEncodingFilter，然后再配置HiddenHttpMethodFilter。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=yicMV&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 使用RESTFul实现用户管理系统
## 静态页面准备
文件包括：user.css、user_index.html、user_list.html、user_add.html、user_edit.html。代码如下：
### user.css
```css
.header {
  background-color: #f2f2f2;
  padding: 20px;
  text-align: center;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}

li {
  float: left;
}

li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}

li a:hover:not(.active) {
  background-color: #111;
}

.active {
  background-color: #4CAF50;
}

form {
  width: 50%;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

label {
  display: block;
  margin-bottom: 8px;
}

input[type="text"], input[type="email"], select {
  width: 100%;
  padding: 6px 10px;
  margin: 8px 0;
  box-sizing: border-box;
  border: 1px solid #555;
  border-radius: 4px;
  font-size: 16px;
}

button[type="submit"] {
  padding: 10px;
  background-color: #4CAF50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color: #3e8e41;
}

table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

.header {
  background-color: #f2f2f2;
  padding: 20px;
  text-align: center;
}

a {
  text-decoration: none;
  color: #333;
}

.add-button {
  margin-bottom: 20px;
  padding: 10px;
  background-color: #4CAF50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #3e8e41;
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=AtQjn&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### user_index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>用户管理系统</title>
  <link rel="stylesheet" href="user.css" type="text/css"></link>
</head>
<body>
  <div class="header">
    <h1>用户管理系统</h1>
  </div>
  <ul>
    <li><a class="active" href="user_list.html">用户列表</a></li>
  </ul>
</body>
</html>
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920283042-90741c89-a7c5-4270-a485-4fcbf8dfc64d.png#averageHue=%23cecece&clientId=u2acf3b8a-91ff-4&from=paste&height=238&id=u80f4956c&originHeight=238&originWidth=1906&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9194&status=done&style=none&taskId=u419fa0f2-03c3-4a41-90be-919a2d124cc&title=&width=1906)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=sWjlz&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### user_list.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>用户列表</title>
  <link rel="stylesheet" href="user.css" type="text/css"></link>
</head>
<body>
  <div class="header">
    <h1>用户列表</h1>
  </div>
  <div class="add-button-wrapper">
    <a class="add-button" href="user_add.html">新增用户</a>
  </div>
  <table>
    <thead>
      <tr>
        <th>编号</th>
        <th>用户名</th>
        <th>性别</th>
        <th>邮箱</th>
        <th>操作</th>
      </tr>
    </thead>
	<tbody>
      <tr>
        <td>1</td>
        <td>张三</td>
        <td>男</td>
        <td>zhangsan@powernode.com</td>
        <td>
          修改
          删除
        </td>
      </tr>
      <tr>
        <td>2</td>
        <td>李四</td>
        <td>女</td>
        <td>lisi@powernode.com</td>
        <td>
          修改
          删除
        </td>
      </tr>
    </tbody>
  </table>
</body>
</html>
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920323233-1a150538-a36d-4a27-8dcb-1ca341b97966.png#averageHue=%23f3f3f3&clientId=u2acf3b8a-91ff-4&from=paste&height=285&id=u8ee93d80&originHeight=285&originWidth=1915&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17799&status=done&style=none&taskId=uf055c9c4-5948-4051-9f4a-80a5d64d098&title=&width=1915)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=YyJz9&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### user_add.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>新增用户</title>
  <link rel="stylesheet" href="user.css" type="text/css"></link>
</head>
<body>
  <h1>新增用户</h1>
  <form>
    <label>用户名:</label>
    <input type="text" name="username" required>

    <label>性别:</label>
    <select name="gender" required>
      <option value="">-- 请选择 --</option>
      <option value="1">男</option>
      <option value="0">女</option>
    </select>

    <label>邮箱:</label>
    <input type="email" name="email" required>

	<button type="submit">保存</button>
  </form>
</body>
</html>
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920360777-8a6a18b4-c642-466f-9291-25544643afab.png#averageHue=%23fcfcfc&clientId=u2acf3b8a-91ff-4&from=paste&height=440&id=udcd61043&originHeight=440&originWidth=1579&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12104&status=done&style=shadow&taskId=u0bff44de-a101-4789-a3df-8d27f1d2f51&title=&width=1579)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=jJ2RQ&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### user_edit.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>修改用户</title>
  <link rel="stylesheet" href="user.css" type="text/css"></link>
</head>
<body>
  <h1>修改用户</h1>
  <form>
    <label>用户名:</label>
    <input type="text" name="username" value="张三" required>

    <label>性别:</label>
    <select name="gender" required>
      <option value="">-- 请选择 --</option>
      <option value="1" selected>男</option>
      <option value="0">女</option>
    </select>

    <label>邮箱:</label>
    <input type="email" name="email" value="zhangsan@powernode.com" required>

    <button type="submit">修改</button>
  </form>
</body>
</html>
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920389489-92688713-932b-40e2-8d8d-a626c6187c5d.png#averageHue=%23fcfcfc&clientId=u2acf3b8a-91ff-4&from=paste&height=448&id=u6318603d&originHeight=448&originWidth=1507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14895&status=done&style=shadow&taskId=u955f5f2e-e418-48b2-81da-23d9f262c0d&title=&width=1507)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=oxCpJ&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## SpringMVC环境搭建
### 创建module：usermgt
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920713139-04e3d84e-1488-42cc-b936-8de84605d590.png#averageHue=%23f2f5f8&clientId=u2acf3b8a-91ff-4&from=paste&height=695&id=u60dfcb70&originHeight=695&originWidth=781&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73722&status=done&style=shadow&taskId=ub23afa55-bdcf-4e62-aae3-6cd376274c0&title=&width=781)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.powernode</groupId>
    <artifactId>usermgt</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <dependencies>
        <!--springmvc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>6.1.5</version>
        </dependency>
        <!--servlet api-->
        <dependency>
            <groupId>jakarta.servlet</groupId>
            <artifactId>jakarta.servlet-api</artifactId>
            <version>6.0.0</version>
        </dependency>
        <!--logback-->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.5.3</version>
        </dependency>
        <!--thymeleaf+spring6整合依赖-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring6</artifactId>
            <version>3.1.2.RELEASE</version>
        </dependency>
    </dependencies>
    
    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=XdEjI&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### 添加web支持
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920903870-9f597c85-e33b-4d65-aebe-5bfb4a6228f2.png#averageHue=%23eff2f9&clientId=u2acf3b8a-91ff-4&from=paste&height=191&id=u799421d7&originHeight=191&originWidth=316&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9637&status=done&style=shadow&taskId=u79c7b42f-e898-4d05-a6f1-e44b0f2b9b8&title=&width=316)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920974114-73d0c44a-3f95-44d6-9abc-cb4db7ff8ab4.png#averageHue=%23f2f5f9&clientId=u2acf3b8a-91ff-4&from=paste&height=430&id=ud1835f71&originHeight=430&originWidth=1289&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32823&status=done&style=shadow&taskId=u40194893-89d9-44cb-bc5e-eec58c71b60&title=&width=1289)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Eh2Y3&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### 配置web.xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
         version="6.0">

    <!--字符编码过滤器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!--HTTP请求方式过滤器-->
    <filter>
        <filter-name>hiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>hiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!--前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```
注意两个过滤器Filter的配置顺序：

- 先配置 CharacterEncodingFilter
- 再配置 HiddenHttpMethodFilter

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=OloDd&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### 配置springmvc.xml文件
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710921461366-720312de-7289-4ea8-98d1-67689d0d17d0.png#averageHue=%23f1f3f6&clientId=u2acf3b8a-91ff-4&from=paste&height=264&id=u1d1dbdbf&originHeight=264&originWidth=290&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14567&status=done&style=shadow&taskId=u604426fb-a6e0-4353-8e60-9fe39eeb4a3&title=&width=290)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.powernode.usermgt.controller,com.powernode.usermgt.dao"/>

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

    <!--开启注解-->
    <mvc:annotation-driven/>

    <!--开启默认Servlet-->
    <mvc:default-servlet-handler/>

</beans>
```
在WEB-INF目录下新建：thymeleaf目录
创建package：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710921862253-344bdb43-894d-4a6d-8bac-8d55b7698ee2.png#averageHue=%23eaeff9&clientId=ueecd5d94-bdb3-4&from=paste&height=359&id=u52b08ecc&originHeight=359&originWidth=365&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22288&status=done&style=shadow&taskId=udd15c6d7-bb54-45b4-aeeb-3359889d277&title=&width=365)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=zadAP&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 显示首页
在应用的根下新建目录：static，将user.css文件拷贝进去。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922471241-f7ec47fc-9106-4d52-bd88-24b1005e99c6.png#averageHue=%23f0f3f8&clientId=ueecd5d94-bdb3-4&from=paste&height=266&id=u19a749a5&originHeight=266&originWidth=303&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12906&status=done&style=none&taskId=ud8c439de-1859-4811-9c8a-3e76047b0c6&title=&width=303)
将user_index.html拷贝到WEB-INF/thymeleaf目录下：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922711285-f6d7e3ea-ee9f-4b95-a454-0f1b41204a46.png#averageHue=%23f1f3f8&clientId=ueecd5d94-bdb3-4&from=paste&height=314&id=u413f67b2&originHeight=314&originWidth=309&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16383&status=done&style=none&taskId=ua0a70d35-1a53-4b72-acbe-2658bc579a3&title=&width=309)
代码有两处需要修改：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922744668-a8863a20-0635-4c69-b461-a182949678d6.png#averageHue=%23fdfbfa&clientId=ueecd5d94-bdb3-4&from=paste&height=446&id=u16a044a8&originHeight=446&originWidth=828&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46389&status=done&style=shadow&taskId=u5b0623fd-571d-4ab2-a97a-c580cbdc40a&title=&width=828)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=hUMid&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

重要：在springmvc.xml文件中配置视图控制器映射：
```xml
<!--视图控制器映射-->
<mvc:view-controller path="/" view-name="user_index"/>
```

部署，启动服务器，测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922946129-cfb0cded-a7de-4b37-9f89-38b01b642655.png#averageHue=%23d3d3d2&clientId=ueecd5d94-bdb3-4&from=paste&height=293&id=u63e0b047&originHeight=293&originWidth=1915&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16700&status=done&style=shadow&taskId=u1c687010-5973-4147-b03b-c8fa57af173&title=&width=1915)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=iIOga&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 实现用户列表
修改user_index.html中的超链接：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>用户管理系统</title>
  <link rel="stylesheet" th:href="@{/static/user.css}" type="text/css"></link>
</head>
<body>
  <div class="header">
    <h1>用户管理系统</h1>
  </div>
  <ul>
    <li><a class="active" th:href="@{/user}">用户列表</a></li>
  </ul>
</body>
</html>
```
编写bean：User
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710923401402-6141a9cd-a92c-48c8-82a0-4822245a5f5c.png#averageHue=%23eaeff9&clientId=ueecd5d94-bdb3-4&from=paste&height=171&id=u4e06c844&originHeight=171&originWidth=327&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9266&status=done&style=shadow&taskId=u3225d6ad-9fb1-4012-bf42-4e88578e981&title=&width=327)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=PtBmk&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
```java
package com.powernode.usermgt.bean;

public class User {
    private Long id;
    private String name;
    private String email;
    private Integer gender;

    public User() {
    }

    public User(Long id, String name, String email, Integer gender) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.gender = gender;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", gender=" + gender +
                '}';
    }
}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=enKY6&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
编写UserDao，提供selectAll方法：
```java
package com.powernode.usermgt.dao;

import com.powernode.usermgt.bean.User;
import org.springframework.stereotype.Repository;

import java.util.ArrayList;
import java.util.List;

@Repository
public class UserDao {
    private static List<User> users = new ArrayList<>();
    static {
        User user1 = new User(10001L, "张三", "zhangsan@powernode.com", 1);
        User user2 = new User(10002L, "李四", "lisi@powernode.com", 1);
        User user3 = new User(10003L, "王五", "wangwu@powernode.com", 1);
        User user4 = new User(10004L, "赵六", "zhaoliu@powernode.com", 0);
        User user5 = new User(10005L, "钱七", "qianqi@powernode.com", 0);
        users.add(user1);
        users.add(user2);
        users.add(user3);
        users.add(user4);
        users.add(user5);
    }

    public List<User> selectAll(){
        return users;
    }
}

```
编写控制器UserController：
```java
package com.powernode.usermgt.controller;

import com.powernode.usermgt.bean.User;
import com.powernode.usermgt.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;

@Controller
public class UserController {

    @Autowired
    private UserDao userDao;

    @GetMapping("/user")
    public String list(Model model){
        // 获取所有的用户
        List<User> users = userDao.selectAll();
        // 存储到request域
        model.addAttribute("users", users);
        // 跳转视图
        return "user_list";
    }
}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Nx4MN&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
将user_list.html拷贝到thymeleaf目录下，并进行代码修改，显示用户列表：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>用户列表</title>
  <link rel="stylesheet" th:href="@{/static/user.css}" type="text/css"></link>
</head>
<body>
  <div class="header">
    <h1>用户列表</h1>
  </div>
  <div class="add-button-wrapper">
    <a class="add-button" href="user_add.html">新增用户</a>
  </div>
  <table>
    <thead>
      <tr>
        <th>编号</th>
        <th>用户名</th>
        <th>性别</th>
        <th>邮箱</th>
        <th>操作</th>
      </tr>
    </thead>
	<tbody>

      <tr th:each="user : ${users}">
        <td th:text="${user.id}"></td>
        <td th:text="${user.name}"></td>
        <td th:text="${user.gender == 1 ? '男' : '女'}"></td>
        <td th:text="${user.email}"></td>
        <td>
          <a href="">修改</a>
          <a href="">删除</a>
        </td>
      </tr>

    </tbody>
  </table>
</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=zf4of&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
测试结果：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710924345455-7ed4d09c-4bf9-4a0f-9c75-a79ac2f19393.png#averageHue=%23f5f4f4&clientId=ueecd5d94-bdb3-4&from=paste&height=447&id=ua72a5c46&originHeight=447&originWidth=1913&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41362&status=done&style=shadow&taskId=u8803d83b-0bdb-46d9-a3c4-e7f8f38f373&title=&width=1913)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=NXX52&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 实现新增功能
### 跳转到新增页面
在用户列表页面，修改`新增用户`的超链接：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710924492210-29f6afb3-551b-478e-adda-4b1952ba2971.png#averageHue=%23fdfbfa&clientId=ueecd5d94-bdb3-4&from=paste&height=188&id=u596104c4&originHeight=188&originWidth=690&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20250&status=done&style=shadow&taskId=u46fae2b1-f40d-49e0-bc30-8c6362171af&title=&width=690)
将user_add.html拷贝到thymeleaf目录下，并进行代码修改如下：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http:www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>新增用户</title>
  <link rel="stylesheet" th:href="@{/static/user.css}" type="text/css"></link>
</head>
<body>
  <h1>新增用户</h1>
  <form>
    <label>用户名:</label>
    <input type="text" name="username" required>

    <label>性别:</label>
    <select name="gender" required>
      <option value="">-- 请选择 --</option>
      <option value="1">男</option>
      <option value="0">女</option>
    </select>

    <label>邮箱:</label>
    <input type="email" name="email" required>

	<button type="submit">保存</button>
  </form>
</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=l6vz5&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

在springmvc.xml文件中配置`视图控制器映射`：
```xml
<mvc:view-controller path="/toSave" view-name="user_add"/>
```
启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710924719699-451900a1-ead4-463f-8536-8db72ac56b0b.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=508&id=u4c32fb33&originHeight=508&originWidth=1589&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18194&status=done&style=shadow&taskId=ufd8edbca-233c-4719-b5a4-80f73b29277&title=&width=1589)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=JxG20&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
### 实现新增功能
前端页面发送POST请求，提交表单，user_add.html代码如下：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http:www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>新增用户</title>
  <link rel="stylesheet" th:href="@{/static/user.css}" type="text/css"></link>
</head>
<body>
  <h1>新增用户</h1>
  <form th:action="@{/user}" method="post">
    <label>用户名:</label>
    <input type="text" name="name" required>

    <label>性别:</label>
    <select name="gender" required>
      <option value="">-- 请选择 --</option>
      <option value="1">男</option>
      <option value="0">女</option>
    </select>

    <label>邮箱:</label>
    <input type="email" name="email" required>

	<button type="submit">保存</button>
  </form>
</body>
</html>
```
编写控制器UserController：
```java
@PostMapping("/user")
public String save(User user){
    // 保存用户
    userDao.save(user);
    // 重定向到列表
    return "redirect:/user";
}
```
**注意：保存成功后，采用重定向的方式跳转到用户列表。**

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=a81D7&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

编写UserDao：
```java
public static Long generateId(){
    // Stream API
    Long maxId = users.stream().map(user -> user.getId()).reduce((id1, id2) -> id1 > id2 ? id1 : id2).get();
    return maxId + 1;
}

public void save(User user){
    // 设置id
    user.setId(generateId());
    // 保存
    users.add(user);
}
```
**注意：单独写了一个方法生成id，内部使用了Stream API，不会这块内容的可以看老杜最新发布的2024版JavaSE。**

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=mzRYp&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710925396950-eb9be9ac-0640-4bef-94c0-3ca586769901.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=479&id=uca8cf260&originHeight=479&originWidth=1490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19503&status=done&style=shadow&taskId=u7a7070b4-45bf-4a5b-ad7c-4e453139494&title=&width=1490)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710925419604-76e61dc5-7490-43e2-8814-9b576f2a2e12.png#averageHue=%23f5f3f3&clientId=ueecd5d94-bdb3-4&from=paste&height=543&id=u44175633&originHeight=543&originWidth=1760&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44843&status=done&style=shadow&taskId=uc16f562f-a1b1-4c94-b019-196e27bacf0&title=&width=1760)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Mxh30&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 跳转到修改页面
修改user_list.html中`修改`超链接：
```html
<a th:href="@{'/user/' + ${user.id}}">修改</a>
```
编写Controller：
```java
@GetMapping("/user/{id}")
public String toUpdate(@PathVariable("id") Long id, Model model){
    // 根据id查询用户信息
    User user = userDao.selectById(id);
    // 将对象存储到request域
    model.addAttribute("user", user);
    // 跳转视图
    return "user_edit";
}
```
编写UserDao：
```java
public User selectById(Long id){
    return users.stream().filter(user -> user.getId().equals(id)).findFirst().get();
}
```
将user_edit.html拷贝thymeleaf目录下，并修改代码如下：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>修改用户</title>
  <link rel="stylesheet" th:href="@{/static/user.css}" type="text/css"></link>
</head>
<body>
  <h1>修改用户</h1>
  <form>
    <label>用户名:</label>
    <input type="text" name="username" th:value="${user.name}" required>

    <label>性别:</label>
    <select name="gender" required>
      <option value="">-- 请选择 --</option>
      <option value="1" th:field="${user.gender}">男</option>
      <option value="0" th:field="${user.gender}">女</option>
    </select>

    <label>邮箱:</label>
    <input type="email" name="email" th:value="${user.email}" required>

    <button type="submit">修改</button>
  </form>
</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=djB9z&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710926069744-03a87f7a-fcb9-4dce-be86-32c7f8ca956d.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=492&id=u9440f8f6&originHeight=492&originWidth=1499&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20205&status=done&style=shadow&taskId=u8ea7a19a-3251-4335-b130-b7620f0b1f2&title=&width=1499)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=fC90i&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 实现修改功能
将user_edit.html页面中的form表单修改一下，添加action，添加method，隐藏域的方式提交请求方式put，隐藏域的方式提交id：
```html
<form th:action="@{/user}" method="post">
  <!--隐藏域的方式设置请求方式为put请求-->
  <input type="hidden" name="_method" value="put">
  <!--隐藏域的方式提交id-->
  <input type="hidden" name="id" th:value="${user.id}">

  <label>用户名:</label>
  <input type="text" name="name" th:value="${user.name}" required>

  <label>性别:</label>
  <select name="gender" required>
    <option value="">-- 请选择 --</option>
    <option value="1" th:field="${user.gender}">男</option>
    <option value="0" th:field="${user.gender}">女</option>
  </select>

  <label>邮箱:</label>
  <input type="email" name="email" th:value="${user.email}" required>

  <button type="submit">修改</button>
</form>
```
编写Controller：
```java
@PutMapping("/user")
public String modify(User user){
    // 更新数据
    userDao.update(user);
    // 重定向
    return "redirect:/user";
}
```
编写UserDao：
```java
public void update(User user){
    for (int i = 0; i < users.size(); i++) {
        if(user.getId().equals(users.get(i).getId())){
            users.set(i, user);
            break;
        }
    }
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=P6Ixd&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710926528699-cec23c03-5c13-4dfb-a3cd-8c6dfcb5457a.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=510&id=u46c7c68a&originHeight=510&originWidth=1558&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21235&status=done&style=shadow&taskId=uca8e0f99-aec4-40d0-8bd9-cbe92b6d11a&title=&width=1558)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710926619799-63c5955e-1933-42b5-b11a-c38c3d3d1213.png#averageHue=%23f6f4f3&clientId=ueecd5d94-bdb3-4&from=paste&height=480&id=u5bab75c0&originHeight=480&originWidth=1791&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41101&status=done&style=none&taskId=ud723fd93-ff49-4627-aae8-818a5631256&title=&width=1791)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=tyXOx&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 实现删除功能
删除应该发送DELETE请求，要模拟DELETE请求，就需要使用表单方式提交。因此我们点击`删除`超链接时需要采用表单方式提交。
在user_list.html页面添加form表单，并且点击超链接时应该提交表单，代码如下：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>用户列表</title>
  <link rel="stylesheet" th:href="@{/static/user.css}" type="text/css"></link>
</head>
<body>
  <div class="header">
    <h1>用户列表</h1>
  </div>
  <div class="add-button-wrapper">
    <a class="add-button" th:href="@{/toSave}">新增用户</a>
  </div>
  <table>
    <thead>
      <tr>
        <th>编号</th>
        <th>用户名</th>
        <th>性别</th>
        <th>邮箱</th>
        <th>操作</th>
      </tr>
    </thead>
	<tbody>

      <tr th:each="user : ${users}">
        <td th:text="${user.id}"></td>
        <td th:text="${user.name}"></td>
        <td th:text="${user.gender == 1 ? '男' : '女'}"></td>
        <td th:text="${user.email}"></td>
        <td>
          <a th:href="@{'/user/' + ${user.id}}">修改</a>
          <!--为删除提供一个鼠标单击事件-->
          <a th:href="@{'/user/' + ${user.id}}" onclick="del(event)">删除</a>
        </td>
      </tr>

    </tbody>
  </table>

  <!--为删除操作准备一个form表单，点击删除时提交form表单-->
  <div style="display: none">
  <form method="post" id="delForm">
    <input type="hidden" name="_method" value="delete"/>
  </form>
  </div>

  <script>
    function del(event){
      // 获取表单
      let delForm = document.getElementById("delForm");
      // 设置表单action
      delForm.action = event.target.href;
      if(window.confirm("您确定要删除吗？")){
        // 提交表单
        delForm.submit();
      }
      // 阻止超链接默认行为
      event.preventDefault();
    }
  </script>
</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Qm0o3&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

编写Controller:
```java
@DeleteMapping("/user/{id}")
public String del(@PathVariable("id") Long id){
    // 删除
    userDao.deleteById(id);
    // 重定向
    return "redirect:/user";
}
```
编写UserDao:
```java
public void deleteById(Long id){
    for (int i = 0; i < users.size(); i++) {
        if(id.equals(users.get(i).getId())){
            users.remove(i);
            break;
        }
    }
}
```
启动服务器测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710929370776-f026da63-52ce-46e7-9928-e3f7be33b089.png#averageHue=%23f6f6f5&clientId=ueecd5d94-bdb3-4&from=paste&height=462&id=u7b2c683f&originHeight=462&originWidth=1795&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46950&status=done&style=shadow&taskId=u6ac162e5-114c-4ddf-b412-acf307586f2&title=&width=1795)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710929387267-a665ee90-8917-4386-9cba-599b5871d164.png#averageHue=%23f4f4f3&clientId=ueecd5d94-bdb3-4&from=paste&height=411&id=u6523602f&originHeight=411&originWidth=1676&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33854&status=done&style=shadow&taskId=ufef65cc7-ec35-4443-8400-72c1a3a3782&title=&width=1676)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=ZviJ6&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
