 # 一、Spring Boot核心注解

创建一个新的模块，来学习Spring Boot核心注解：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729564104331-5d4976ae-092d-405e-a94b-e69daaae31e6.png)

只加入web启动器。

## 1.@SpringBootApplication注解
* **Spring Boot的主入口程序被`@SpringBootApplication`注解标注，可见这个注解的重要性，查看它的源码：**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729563192417-c03008ef-81f9-4741-ad09-42d49a4b2cc9.png)
* **可以看出这个注解属于`组合注解`。拥有`@SpringBootConfiguration`、`@EnableAutoConfiguration`、`@ComponentScan`的功能。**
	


## 2.@SpringBootConfiguration注解
@SpringBootConfiguration注解的源码如下：  

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729563436496-752f56df-52aa-404b-bf83-c122a06f1312.png)
 * **关于 @SpringBootConfiguration 注解**：从源码角度来看，该注解被 @Configuration 注解标注。因此得出一个结论：**springboot项目的主入口类同时又是一个配置类，因此在springboot主入口配置类当中使用 @Bean 注解标注方法的话，该方法的返回值对象应该会被纳入IoC容器的管理。  **
 * 由于我们知道了@SpringBootApplication注解标注的SpringBoot主入口类实际上就是一个配置类。我们可以对main中的下图语句有新的理解：**run方法的第一个参数其实就是配置类，对应的就是以前的配置文件。springboot应用程序就是从这个配置类开始，加载所有的bean的，Springboot304CoreAnnotationApplication.class 又被称为源【起源】**    
	![](assets/03Spring%20Boot核心注解和单元测试/file-20250810225609236.png)

可以看到这个注解的被`@Configuration`标注，说明`主入口`程序是一个配置类。也就是说主入口中的方法可以被`@Bean`注解标注，被`@Bean`注解的标注的方法会被Spring容器自动调用，并且将该方法的返回对象纳入IoC容器的管理。测试一下：

```java
@SpringBootApplication
public class Sb305CoreApplication {
    @Bean
    public Date getNowDate(){ // 方法名作为bean的id
        return new Date();
    }
    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(Sb305CoreApplication.class, args);
        Date dateBean1 = applicationContext.getBean(Date.class);
        System.out.println(dateBean1);
        Date dateBean2 = applicationContext.getBean("getNowDate", Date.class);
        System.out.println(dateBean2);
    }
}  
```

执行结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729564458157-1d953623-9405-4577-8fa4-ed955038be77.png)

这个`配置类`也可以称为  `源`，起源的意思，SpringBoot从这个配置类开始加载项目中所有的bean。





## 3.@EnableAutoConfiguration注解

* **该注解表示`启用自动配置`。也就是说默认情况下，springboot应用都会默认启用自动配置**
* **Spring Boot 会根据你引入的依赖自动为你配置好一系列的 Bean，无需手动编写复杂的配置代码。**
* **自动配置：只要启动，springboot应用会去类路径当中查找class，根据类路径当中有某个类，或引入了某些启动器（依赖），来自动管理bean，不需要我们程序员手动配置**

例如：如果你在SpringBoot项目的applicaiton.properties中进行了如下配置：
  
```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=root
spring.datasource.password=123456
```

并且在依赖中引入了`mybatis依赖`/`mybatis启动器`，那么SpringBoot框架将为你自动化配置以下bean：
+ **SqlSessionFactory**: MyBatis的核心工厂SqlSessionFactory会被自动配置。这个工厂负责创建SqlSession实例，后者用来执行映射文件中的SQL语句。
+ **TransactionManager**: DataSourceTransactionManager会被自动配置来管理与数据源相关的事务。

使用下面的程序测试是否进行了自动配置

![](assets/03Spring%20Boot核心注解和单元测试/file-20250810232514884.png)
* **主入口程序main()中的run方法是与返回值的，返回值是ConfigurableApplicationContext，其 继承了 ApplicationContext，所以因此 run方法的返回值就是spring容器。**
* **通过bean的name获取bean,默认是类名首字母小写。**
* 如果最后不调用spring容器的close()方法，服务器程序则一直执行。但是如果写了关闭代码，则相当于执行完main方法就直接关闭



## 4.@ComponentScan注解
这个注解的作用是：启动组件扫描功能，代替spring框架xml文件中这个配置：

```xml
<context:component-scan base-package="com.powernode.sb305core"/>
```

* **因此被`@SpringBootApplication`注解标注之后，会启动组件扫描功能，组件扫描默认扫描的包是主入口程序所在的包以及该包下的所有子包。因此如果一个bean要纳入IoC容器的管理则必须放到主入口程序所在包及子包下**。放到主入口程序所在包之外的话，扫描不到。测试一下：

### 扫描到
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729565961938-3cbe2276-79a3-4a57-ab29-d2cd3153d5f8.png)

`HelloController`代码如下：

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(){
        return "hello world!";
    }
}
```

启动服务器测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729566015788-bfbce42f-90b2-48cf-b583-4490db6da625.png)





### 扫描不到
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729566119617-fd950fb7-d8a1-42f7-8ff0-e1d7fab60409.png)

可以看到`UserController`没有在`sb305core`包下。



`UserController`代码如下：

```java
@RestController
public class UserController {
    @GetMapping("/list")
    public String list(){
        return "user list!";
    }
}
```

启动服务器测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729566187896-1d4053bd-d5da-4fd4-a243-ebd80388ac17.png)

通过测试得知`UserController`没有被纳入IoC容器的管理。



最终结论：要让bean纳入IoC容器的管理，必须将类放到主入口程序同级目录下，或者子目录下。





# 二、Spring Boot的单元测试
## 1.不使用单元测试怎么调用service
### 创建模块
使用脚手架创建sb3-06-test模块，不添加任何启动器：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729581295420-d78e7122-66d3-43da-9ba0-a5dea864cf7a.png)

### 编写service
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729581533729-d56983cc-f11b-4f3a-ac12-cbc28989d0b9.png)

```java
package com.powernode.sb306test.service.impl;

import com.powernode.sb306test.service.UserService;
import org.springframework.stereotype.Service;

@Service("userService")
public class UserServiceImpl implements UserService {
    @Override
    public void save() {
        System.out.println("保存用户信息");
    }
}
```


### 直接在入口程序中调用service
```java
@SpringBootApplication
public class Sb306TestApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(Sb306TestApplication.class, args);
        UserService userService = applicationContext.getBean("userService", UserService.class);
        userService.save();
    }
}
```

执行结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729581624703-aa5dfd5f-1a61-48a9-bce3-9d3d615913e5.png)



这种方式就是手动获取Spring上下文对象`ConfigurableApplicationContext`，然后调用getBean方法从Spring容器中获取service对象，然后调用方法。





## 2.使用单元测试怎么调用service
### test-starter引入以及测试类编写
使用单元测试应该如何调用service对象上的方法呢？

在使用脚手架创建Spring Boot项目时，为我们生成了单元测试类，如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729581970332-45ced754-fba5-4f5f-92af-8278690b069c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729581986594-85a62eb9-67d3-4637-a9de-d2578bc1b55f.png)

当然，如果要使用单元测试，需要引入单元测试启动器，如果使用脚手架创建SpringBoot项目，这个test启动器会自动引入：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```



### @SpringBootTest注解
**`@SpringBootTest` 会创建一个完整的 Spring 应用程序上下文（Application Context），这个上下文包含了应用程序的所有组件和服务**。以下是 `@SpringBootTest` 做的一些主要工作：

1. **创建 ApplicationContext**：
    - `@SpringBootTest` 使用 `SpringApplication` 的 `run()` 方法来启动一个 Spring Boot 应用程序上下文。这意味着它会加载应用程序的主配置类和其他相关的配置类。
2. **加载配置文件**：
    - 它会查找并加载默认的配置文件，如 `application.properties`
3. **支持自动配置**：
    - 如果应用程序依赖于 Spring Boot 的自动配置特性，`@SpringBootTest` 会确保这些自动配置生效。这意味着它会根据可用的类和bean来自动配置一些组件，如数据库连接、消息队列等。
4. **注入依赖**：
    - 使用 `@SpringBootTest` 创建的应用程序上下文允许你在测试类中使用 `@Autowired` 注入需要的 bean，就像在一个真实的 Spring Boot 应用程序中一样。


![](assets/03Spring%20Boot核心注解和单元测试/file-20251006112501927.png)

总的来说，`@SpringBootTest` 为你的测试提供了尽可能接近实际运行时环境的条件，这对于验证应用程序的行为非常有用。



### 注入service并调用
```java
@SpringBootTest
class Sb306TestApplicationTests {

    @Autowired
    private UserService userService;
    
    @Test
    void contextLoads() {
        userService.save();
    }

}
```

测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729582782987-88067365-fba6-4240-b704-b2f710a96647.png)



