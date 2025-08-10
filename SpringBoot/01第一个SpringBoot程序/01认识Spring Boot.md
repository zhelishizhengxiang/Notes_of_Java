# 认识Spring Boot
我们来看看官方是如何介绍的：

[https://docs.spring.io/spring-boot/index.html](https://docs.spring.io/spring-boot/index.html)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1728875326069-8146a95b-569b-4338-8d28-696fcb647ec8.png)

翻译：   
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1728893860338-bce86e2c-719d-49fe-9932-419fe1e50fb1.png)
* **springBoot框架中提供了一种机制：自动配置。比如检测你类路径当中是否存在某些类，例如：SqlSessionFactory，这样他就会把mybatis的相关配置自动提供。**


**Spring Boot倡导`约定优于配置`，将`简化开发`发挥到极致**。使用Spring Boot框架可以快速构建Spring应用，再也不需要`大量的繁琐的`的各种配置。**Spring Boot框架设计的目标是：程序员关注业务逻辑就行了，环境方面的事儿交给Spring Boot就行。**


Spring Boot特性：  
1. **快速创建独立的Spring应用程序**。（Spring支持的SpringBoot都支持，也就是说SpringBoot全方位支持IoC，AOP等）
2. **嵌入式的Tomcat、Jetty、Undertow容器**。（web服务器本身就是几个jar包，Spring Boot框架自动嵌入了。）
3. **需要什么功能时只需要引入对应的starter启动器即可**。（启动器可以自动管理这个功能相关的依赖，自动管理依赖版本的控制）
4. **尽最大努力，最大可能的自动配置Spring应用和第三方库**。（例如：如果要进行事务的控制，不用做任何事务相关的配置，只需要在service类上添加@Transactional注解即可。）
5. **没有代码生成，没有XML配置**。（Spring Boot的应用程序在启动后不会动态地创建新的Java类，所有逻辑都是在编译期就已经确定好的）
6. **提供了生产监控的支持，例如健康检查，度量信息，跟踪信息，审计信息等。也支持集成外部监控系统**。



Spring Boot的开箱即用和约定优于配置：  
+ **开箱即用**：Spring Boot框架设计得非常便捷，开发者能够在几乎不需要任何复杂的配置的情况下，快速搭建并运行一个功能完备的Spring应用。
+ **约定优于配置**：“约定优于配置”（Convention Over Configuration, CoC）是一种软件设计哲学，**核心思想是通过提供一组合理的默认行为来减少配置的数量，从而简化开发流程。例如：Spring Boot默认约定了使用某个事务管理器**，在事务方面不需要做任何配置，只需要在对应代码的位置上使用`@Transactional`注解即可。





# 知识储备
学习本套课程的前提是你已经学过了：
+ MyBatis
+ Spring
+ SpringMVC

# 主要版本说明
Spring Boot版本：3.3.5

IDEA版本：2024.2.4

JDK版本：21（Spring Boot3要求JDK的最低版本是JDK17）

