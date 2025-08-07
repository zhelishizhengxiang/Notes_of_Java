## 一、RESTFul方式演示查询

**RESTFul规范中规定，如果要查询数据，需要发送GET请求**。
### 1.根据id查询(GET /api/user/1)
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



### 2.查询所有(GET /api/user)
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


## 二、RESTFul方式演示增加(POST /api/user)
**RESTFul规范中规定，如果要进行保存操作，需要发送POST请求。**
```html
<!--保存用户-->
<form th:action="@{/user}" method="post">  
	用户名：<input type="text" name="username"><br>  
	密码：<input type="password" name="password"><br>  
	年龄：<input type="text" name="age"><br>  
	<input type="submit" value="保存">  
</form>
```

```java
@RequestMapping(value = "/user", method = RequestMethod.POST)  
	public String save(User user) {  
	System.out.println("正在保存用户信息...");  
	System.out.println(user);  
	return "ok";  
}
```

启动服务器测试：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946440909-1140c31e-f921-42fe-bbab-5a9c4409f388.png#averageHue=%23f7f6f6&clientId=u056440f2-a263-4&from=paste&height=212&id=u9585b4cb&originHeight=212&originWidth=395&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11521&status=done&style=shadow&taskId=u0e442a6e-4100-4ac5-b294-d78f1c63333&title=&width=395)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946457841-33c623d2-75a6-4486-bfb0-472e9f3ae72e.png#averageHue=%23fafaf9&clientId=u056440f2-a263-4&from=paste&height=123&id=u724fe648&originHeight=123&originWidth=417&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6348&status=done&style=shadow&taskId=uc8c798af-dbfd-4d55-b487-7ff2a7b161d&title=&width=417)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710946468943-5ecc05c9-cc83-47b7-966b-8fe0df99a3f5.png#averageHue=%23f6f3f1&clientId=u056440f2-a263-4&from=paste&height=108&id=ude85b2fc&originHeight=108&originWidth=247&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6746&status=done&style=shadow&taskId=u858b1229-a0ce-4759-8d5a-aa35ff1fc3d&title=&width=247)


## 三、RESTFul方式演示修改和删除
* **RESTFul规范中规定，如果要进行保存操作，需要发送PUT请求**。
* **如何发送PUT请求？**  
1. **首先你必须是一个POST请求。**
2. **第二步：在发送POST请求的时候，提交这样的数据：**`**_method=PUT/put**`   
	![](assets/02RESTFul方式演示增删改查/file-20250806153931793.png)
3. **第三步：在web.xml文件配置SpringMVC提供的过滤器：HiddenHttpMethodFilter。这个过滤器是springmvc提前写好的，直接用就行了，这个过滤器可以帮助你将请求POST转换成PUT请求/DELETE请求**  
4. **在Controller中设置请求方法方式为put或者是delete**    
	![](assets/02RESTFul方式演示增删改查/file-20250806154138306.png)

注：正常情况下的请求方式如下：  
![](assets/02RESTFul方式演示增删改查/file-20250806153752895.png)

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

