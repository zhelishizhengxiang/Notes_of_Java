
## 一、静态页面准备
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

## 二、SpringMVC环境搭建
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


### 添加web支持
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920903870-9f597c85-e33b-4d65-aebe-5bfb4a6228f2.png#averageHue=%23eff2f9&clientId=u2acf3b8a-91ff-4&from=paste&height=191&id=u799421d7&originHeight=191&originWidth=316&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9637&status=done&style=shadow&taskId=u79c7b42f-e898-4d05-a6f1-e44b0f2b9b8&title=&width=316)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710920974114-73d0c44a-3f95-44d6-9abc-cb4db7ff8ab4.png#averageHue=%23f2f5f9&clientId=u2acf3b8a-91ff-4&from=paste&height=430&id=ud1835f71&originHeight=430&originWidth=1289&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32823&status=done&style=shadow&taskId=u40194893-89d9-44cb-bc5e-eec58c71b60&title=&width=1289)
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


## 三、显示首页
在应用的根下新建目录：static，将user.css文件拷贝进去。  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922471241-f7ec47fc-9106-4d52-bd88-24b1005e99c6.png#averageHue=%23f0f3f8&clientId=ueecd5d94-bdb3-4&from=paste&height=266&id=u19a749a5&originHeight=266&originWidth=303&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12906&status=done&style=none&taskId=ud8c439de-1859-4811-9c8a-3e76047b0c6&title=&width=303)

将user_index.html拷贝到WEB-INF/thymeleaf目录下：   
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922711285-f6d7e3ea-ee9f-4b95-a454-0f1b41204a46.png#averageHue=%23f1f3f8&clientId=ueecd5d94-bdb3-4&from=paste&height=314&id=u413f67b2&originHeight=314&originWidth=309&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16383&status=done&style=none&taskId=ua0a70d35-1a53-4b72-acbe-2658bc579a3&title=&width=309)

代码有两处需要修改：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922744668-a8863a20-0635-4c69-b461-a182949678d6.png#averageHue=%23fdfbfa&clientId=ueecd5d94-bdb3-4&from=paste&height=446&id=u16a044a8&originHeight=446&originWidth=828&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46389&status=done&style=shadow&taskId=u5b0623fd-571d-4ab2-a97a-c580cbdc40a&title=&width=828)



重要：在springmvc.xml文件中配置视图控制器映射：
```xml
<!--视图控制器映射-->
<mvc:view-controller path="/" view-name="user_index"/>
```

部署，启动服务器，测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710922946129-cfb0cded-a7de-4b37-9f89-38b01b642655.png#averageHue=%23d3d3d2&clientId=ueecd5d94-bdb3-4&from=paste&height=293&id=u63e0b047&originHeight=293&originWidth=1915&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16700&status=done&style=shadow&taskId=u1c687010-5973-4147-b03b-c8fa57af173&title=&width=1915)

## 四、实现用户列表
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


测试结果：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710924345455-7ed4d09c-4bf9-4a0f-9c75-a79ac2f19393.png#averageHue=%23f5f4f4&clientId=ueecd5d94-bdb3-4&from=paste&height=447&id=ua72a5c46&originHeight=447&originWidth=1913&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41362&status=done&style=shadow&taskId=u8803d83b-0bdb-46d9-a3c4-e7f8f38f373&title=&width=1913)

## 五、实现新增功能
### 跳转到新增页面（只是页面跳转，配置页面控制器）
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



在springmvc.xml文件中配置`视图控制器映射`：
```xml
<mvc:view-controller path="/toSave" view-name="user_add"/>
```
启动服务器测试：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710924719699-451900a1-ead4-463f-8536-8db72ac56b0b.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=508&id=u4c32fb33&originHeight=508&originWidth=1589&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18194&status=done&style=shadow&taskId=ufd8edbca-233c-4719-b5a4-80f73b29277&title=&width=1589)

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


启动服务器测试：   
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710925396950-eb9be9ac-0640-4bef-94c0-3ca586769901.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=479&id=uca8cf260&originHeight=479&originWidth=1490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19503&status=done&style=shadow&taskId=u7a7070b4-45bf-4a5b-ad7c-4e453139494&title=&width=1490)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710925419604-76e61dc5-7490-43e2-8814-9b576f2a2e12.png#averageHue=%23f5f3f3&clientId=ueecd5d94-bdb3-4&from=paste&height=543&id=u44175633&originHeight=543&originWidth=1760&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44843&status=done&style=shadow&taskId=uc16f562f-a1b1-4c94-b019-196e27bacf0&title=&width=1760)

## 六、跳转到修改页面
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



启动服务器测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710926069744-03a87f7a-fcb9-4dce-be86-32c7f8ca956d.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=492&id=u9440f8f6&originHeight=492&originWidth=1499&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20205&status=done&style=shadow&taskId=u8ea7a19a-3251-4335-b130-b7620f0b1f2&title=&width=1499)

## 七、实现修改功能
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
 
启动服务器测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710926528699-cec23c03-5c13-4dfb-a3cd-8c6dfcb5457a.png#averageHue=%23fcfcfc&clientId=ueecd5d94-bdb3-4&from=paste&height=510&id=u46c7c68a&originHeight=510&originWidth=1558&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21235&status=done&style=shadow&taskId=uca8e0f99-aec4-40d0-8bd9-cbe92b6d11a&title=&width=1558)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710926619799-63c5955e-1933-42b5-b11a-c38c3d3d1213.png#averageHue=%23f6f4f3&clientId=ueecd5d94-bdb3-4&from=paste&height=480&id=u5bab75c0&originHeight=480&originWidth=1791&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41101&status=done&style=none&taskId=ud723fd93-ff49-4627-aae8-818a5631256&title=&width=1791)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=tyXOx&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 八、实现删除功能
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

