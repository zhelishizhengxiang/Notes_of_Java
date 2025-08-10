* **在 Spring Boot 中，启动器（Starter）本质上是一个简化依赖管理的概念**。
* S**pring Boot 的启动器本质上就是一组预定义的依赖集合**，它们被组织成一个个 Maven的依赖，以方便开发者快速集成特定的功能模块。
* **如果你想做web开发，只需要引入web启动器。web启动器会自动引入web开发所需要的子依赖。**
## 1.启动器实现原理
1. **依赖聚合**：  
每个启动器通常对应一个特定的功能集或者一个完整的应用模块，如 `spring-boot-starter-web` 就包含了构建 Web 应用所需的所有基本依赖项，如 Spring MVC, Tomcat 嵌入式容器等。
2. **依赖传递**：  
当你在项目中引入一个启动器时，它不仅会把自身作为依赖加入到你的项目中，还会把它的所有直接依赖项（transitive dependencies）也加入进来。这意味着你不需要单独声明这些依赖项，它们会自动成为项目的一部分。
3. **版本管理**：  
启动器内部已经指定了所有依赖项的具体版本，这些版本信息存储在一个公共的 BOM（Bill of Materials，物料清单）文件中，通常是 `spring-boot-dependencies`。当引入启动器时，实际上也间接引用了这个 BOM，从而确保了所有依赖项版本的一致性。
4. **自动配置**：  
许多启动器还提供了自动配置（Auto-configuration），这是一种机制，允许 Spring Boot 根据类路径上的可用组件自动设置你的应用程序。例如，如果类路径上有 Spring MVC 和嵌入式 Tomcat，则 Spring Boot 会自动配置它们，并准备好一个 web 应用程序。




**使用启动器的示例**

假设你想创建一个基于 Spring MVC 的 RESTful Web 应用，你可以简单地将 `spring-boot-starter-web` 添加到你的项目中：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

当你添加这个依赖时，Spring Boot 会处理所有必要的细节，包括添加 Spring MVC 和 Tomcat 作为嵌入式 Servlet 容器，并且根据类路径上的内容进行适当的自动配置。如下图所示：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729327501374-2bf74e3a-cbc8-4c66-b206-dd38fec8a251.png)

这就是 Spring Boot 启动器的基本实现原理，它简化了依赖管理，让开发者能够更专注于业务逻辑的实现。





## 2.都有哪些启动器

启动器通常包括：
+ **SpringBoot官方提供的启动器**
+ **非官方提供的启动器**

### 官方提供的启动器
**启动器命名特点：spring-boot-starter-\***

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729328350698-5231f924-4ae0-447b-a1af-f2c25e4f8440.png)




### 非官方的启动器
启动器命名特点：**\*-spring-boot-starter**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729328504925-f0aa6730-ee7f-4a1d-85f9-3d2c9075318f.png)



