
准备工作：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.powernode</groupId>
    <artifactId>spring6-015-junit</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <!--仓库-->
    <repositories>
        <!--spring里程碑版本的仓库-->
        <repository>
            <id>repository.spring.milestone</id>
            <name>Spring Milestone Repository</name>
            <url>https://repo.spring.io/milestone</url>
        </repository>
    </repositories>

    <dependencies>
        <!--spring context依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--spring对junit的支持相关依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--junit4依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```
```java
package com.powernode.spring6.bean;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/**
 * @author 动力节点
 * @version 1.0
 * @className User
 * @since 1.0
 **/
@Component
public class User {

    @Value("张三")
    private String name;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public User() {
    }

    public User(String name) {
        this.name = name;
    }
}

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.powernode.spring6.bean"/>
</beans>
```
单元测试：
```java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * @author 动力节点
 * @version 1.0
 * @className SpringJUnit4Test
 * @since 1.0
 **/
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:spring.xml")
public class SpringJUnit4Test {

    @Autowired
    private User user;

    @Test
    public void testUser(){
        System.out.println(user.getName());
    }
}

```
执行结果如下：   
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666602069724-ec6288cc-bb7b-417e-995e-8e1978ee6943.png#averageHue=%23f8f4f1&clientId=uf3bdf93c-900b-4&from=paste&height=240&id=u13070b41&originHeight=240&originWidth=559&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29892&status=done&style=shadow&taskId=u053c1ac2-b862-4efe-87f8-7e1e95bd741&title=&width=559)

Spring提供的方便主要是这几个注解：
* **@RunWith(SpringJUnit4ClassRunner.class)**
* **@ContextConfiguration("classpath:spring.xml")**
* **在单元测试类上使用这两个注解之后，他就会通过注解的方式去加载spring容器，并且可以在单元测试类中的属性上可以使用@Autowired。比较方便**。
