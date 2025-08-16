
新建项目`sb3-09-web`：添加web启动器，添加Lombok依赖。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729842528256-345582ac-9fbe-464f-9d1a-3ee7800bea8a.png)



## 1.web自动配置的依赖是如何传递的
1. 首先引入了`web启动器`，如下：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

2. `web启动器`传递引入了`spring-boot-starter`，如下：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>3.3.5</version>
  <scope>compile</scope>
</dependency>
```

3. `spring-boot-starter`会传递引入一个`spring-boot-autoconfigure`包，如下：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-autoconfigure</artifactId>
  <version>3.3.5</version>
  <scope>compile</scope>
</dependency>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729843303324-6c665ade-1fc3-465c-b11b-0ac167c5bd4c.png)



4. 在`spring-boot-autoconfigure`包中的`.imports`文件中罗列的需要导入的自动配置类，如下图：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729843518728-be8a441f-d6cc-4781-8f0e-a3c67a05b697.png)


## 2.web自动配置的实现原理
1. 从入口程序开始：

```java
package com.powernode.sb309web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Sb309WebApplication {

    public static void main(String[] args) {
        SpringApplication.run(Sb309WebApplication.class, args);
    }
}
```

入口程序被`@SpringBootApplication`注解标注。

2. `@SpringBootApplication`注解被`@EnableAutoConfiguration`<font style="color:#080808;background-color:#ffffff;">注解标注。表示启用自动配置。</font>
3. `@EnableAutoConfiguration`<font style="color:#080808;background-color:#ffffff;">注解被</font>`@Import({AutoConfigurationImportSelector.class})`<font style="color:#080808;background-color:#ffffff;">注解标注。</font>
4. <font style="color:#080808;background-color:#ffffff;">因此</font>`AutoConfigurationImportSelector`<font style="color:#080808;background-color:#ffffff;">决定哪些自动配置类是需要导入的。</font>
5. AutoConfigurationImportSelector底层实现步骤具体如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845866175-609a5c89-9342-4d96-b624-fcc5a7dc2434.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845887890-372b80d6-2e7c-4d7e-8c5c-40cc3402da91.png)


![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845909271-f6640224-ae46-4d74-bea2-ff00fa4e95d7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845939380-9158c6fe-9ac2-43e3-8b4a-fb9b568eefd3.png)

最终找的文件是：`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`

**注意：任何jar包，包括第三方的依赖，自动配置类所在的路径以及文件名都是完全相同的，都是**`**META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports**`

例如mybatis的自动配置类的列表文件也是这样，如下图：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729846670846-84850d75-8a0f-418c-8c64-f48f00f3e738.png)



## 3.通过web自动配置类逆推web配置的prefix
在自动配置列表中找到web自动配置相关的类：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729846834747-246f3a88-f822-46bc-a00c-7770d52c5c26.png)



以下就是web自动配置类列表：

```java
org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration
org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration
```






通过web自动配置类的源码可以逆推web配置的prefix：

1. **WebMvcAutoConfiguration**：web开发的自动配置类

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847170017-0f711f67-f266-4ac2-a25a-c239475b38fe.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847235858-7cab9d78-465f-4289-be9c-9b399570e8d3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847268539-715a377a-ecb9-48fc-ad58-9194d23b7cdf.png)



2. MultipartAutoConfiguration：实现文件上传的配置类

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847417032-2abad318-a48b-49a7-a2dc-26abd0f4d8e5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847475254-4f486638-6132-40c0-baee-25c4c3091f49.png)



3. HttpEncodingAutoConfiguration

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847580938-8114e69c-ba29-4305-ba80-3b0d277be2e1.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847644668-2d8aa37a-699d-4009-92c7-3c9a60efcb49.png)



4. ErrorMvcAutoConfiguration
5. ServletWebServerFactoryAutoConfiguration
6. DispatcherServletAutoConfiguration
7. EmbeddedWebServerFactoryCustomizerAutoConfiguration
8. RestTemplateAutoConfiguration



通过查看源码，得知，web开发时，在`application.properties`配置文件中可以配置的前缀是：

```properties
# SpringMVC相关配置
spring.mvc.

# web开发通用配置
spring.web.

# 文件上传配置
spring.servlet.multipart.

# 服务器配置
server.
```



## 4.Web自动配置都默认配置了什么
查看官方文档：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731383415638-90d102e9-1fd0-4f31-9736-9e3bef3936d3.png)



**翻译如下：**

Spring Boot 为 Spring MVC 提供了自动配置，这在大多数应用程序中都能很好地工作。除了已经实现了 Spring MVC 的默认功能外，自动配置还提供了以下特性：

+ 包括 `ContentNegotiatingViewResolver` 和 `BeanNameViewResolver` 的 Bean。
    - `ContentNegotiatingViewResolver` 自动根据HTTP请求头中Accept字段来选择合适的视图技术渲染响应。
    - BeanNameViewResolver的作用是根据视图名称找到视图View对象。
+ 支持提供静态资源，包括对 WebJars的支持。
    - 静态资源路径默认已经配置好了。默认会去static目录下找。
+ 自动注册 `Converter`、`GenericConverter` 和 `Formatter` 的 Bean。
    - `Converter`：转换器，做类型转换的，例如表单提交了用户数据，将表单数据转换成User对象。
    - `Formatter`：格式化器，做数据格式化的，例如将Java中的`日期类型对象`格式化为`特定格式的日期字符串`。或者将用户提交的日期字符串，转换为Java中的日期对象。
+ 支持 `HttpMessageConverters`。
    - 内置了很多的HTTP消息转换器。例如：`MappingJackson2HttpMessageConverter`可以将json转换成java对象，也可以将java对象转换为json字符串。
+ 自动注册 `MessageCodesResolver`。
    - SpringBoot会自动注册一个默认的`消息代码解析器`
    - 帮助你在表单验证出错时生成一些特殊的代码。这些代码让你能够更精确地定位问题，并提供更友好的错误提示。
+ 静态 `index.html` 文件支持。
    - Spring Boot 会自动处理位于项目静态资源目录下的 index.html 文件，使其成为应用程序的默认主页
+ 自动使用 `ConfigurableWebBindingInitializer` Bean。
    - 用它来指定默认使用哪个转换器，默认使用哪个格式化器。在这个类当中都已经配好了。

* **如果您不想使用自动配置并希望完全控制 Spring MVC，可以添加您自己的带有  @EnableWebMvc注解的 `@Configuration`**。

* **如果您希望保留这些 Spring Boot MVC 定制化设置并进行更多的 MVC 定制化（如拦截器、格式化程序、视图控制器等其他功能），可以添加您自己的实现了 `WebMvcConfigurer` 接口的 `@Configuration` 类。但不能使用@EnableWebMvc注解**






## 5.WebMvcAutoConfiguration原理
通过源码分析的方式，学习WebMvc的自动配置原理。

### WebMvc自动配置是否生效的条件
```java
@AutoConfiguration(after = { DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
		ValidationAutoConfiguration.class })
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@ImportRuntimeHints(WebResourcesRuntimeHints.class)
public class WebMvcAutoConfiguration {}
```

+ @AutoConfiguration(after = { DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,ValidationAutoConfiguration.class })
    - **该注解说明WebMvcAutoConfiguration自动配置类加载顺序在以上自动配置类加载后加载。**
+ @ConditionalOnWebApplication(type = Type.SERVLET)
    - **WebMvcAutoConfiguration自动配置类只在servlet环境中生效。**
+ @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
    - **类路径中必须存在`Servlet.class``DispatcherServlet.class``WebMvcConfigurer.class`，WebMvcAutoConfiguration自动配置类才会生效。**
+ @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
    - **类路径中不存在`WebMvcConfigurationSupport.class`时WebMvcAutoConfiguration自动配置类才会生效。**
    - **注意：当使用@EnableWebMvc注解后，类路径中就会注册一个WebMvcConfigurationSupport这样的bean。**
+ @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10) **不重要**
    - 指定**WebMvcAutoConfiguration自动配置类**的加载顺序
+ @ImportRuntimeHints(WebResourcesRuntimeHints.class) **不重要**
    - 运行时引入**WebResourcesRuntimeHints**这个类，这个类的作用是给JVM或者其他组件提示信息的，提示一下系统应该如何处理类和资源。



总结来说，WebMvcAutoConfiguration类将在以下条件下生效：

1. **应用程序是一个Servlet类型的Web应用；**
2. **环境中有Servlet、DispatcherServlet和WebMvcConfigurer类**；
3. 容器中没有WebMvcConfigurationSupport的bean。

如果这些条件都满足的话，那么这个自动配置类就会被激活，并进行相应的自动配置工作。





### WebMvc自动配置生效后引入了两个Filter Bean
#### 引入了HiddenHttpMethodFilter Bean
```java
@Bean
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled")
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new OrderedHiddenHttpMethodFilter();
}
```

**这个过滤器是专门处理Rest请求的。GET POST PUT DELETE请求**。

#### 引入了FormContentFilter Bean
```java
@Bean
@ConditionalOnMissingBean(FormContentFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.formcontent.filter", name = "enabled", matchIfMissing = true)
public OrderedFormContentFilter formContentFilter() {
    return new OrderedFormContentFilter();
}
```

OrderedFormContentFilter 是 Spring Boot 中用于处理 HTTP 请求的一个过滤器，特别是针对 PUT 和 DELETE 请求。**这个过滤器的主要作用是在处理 PUT 和 DELETE 请求时，确保如果请求体中有表单格式的数据，这些数据会被正确解析并可用。**


 

### WebMvc自动配置生效后引入了WebMvcConfigurer接口的实现类(重点)
在SpringBoot框架的`WebMvcAutoConfiguration`类中提供了一个内部类：`WebMvcAutoConfigurationAdapter`  

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730345630650-62e11666-3e95-4c64-9787-c0b18d083c91.png)

* **SpringBoot在这个类`WebMvcAutoConfigurationAdapter`中进行了一系列的Spring MVC相关配置。**
* **我们开发中要对Spring MVC的相关配置进行修改，可以编写一个类继承WebMvcAutoConfigurationAdatper然后重写对应的方法即可。**
* **因此，通过对WebMvcAutoConfigurationAdapter类中的方法进行重写来修改Web MVC的默认配置。**

**<font style="color:#DF2A3F;"></font>**


#### 关于`WebMvcConfigurer`接口
这个接口不是SpringBoot框架提供的，是Spring MVC提供的，在Spring框架4.3版本中引入的。**这个接口的作用主要是允许开发者通过实现这个接口来定制Spring MVC的行为**。

**在这个接口中提供了很多方法，需要改变Spring MVC的哪个行为，则重写对应的方法即可**，下面是这个接口中所有的方法，以及每个方法对应的Spring MVC行为的解释：

```java
public interface WebMvcConfigurer {
    // 用于定制 Spring MVC 如何匹配请求路径到控制器
    default void configurePathMatch(PathMatchConfigurer configurer) {}
    // 用于定制 Spring MVC 的内容协商策略，以确定如何根据请求的内容类型来选择合适的处理方法或返回数据格式
    default void configureContentNegotiation(ContentNegotiationConfigurer configurer) {}
    // 用于定制 Spring MVC 处理异步请求的方式
    default void configureAsyncSupport(AsyncSupportConfigurer configurer) {}
    // 用于定制是否将某些静态资源请求转发WEB容器默认的Servlet处理
    default void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {}
    // 用于定制 Spring MVC 视图解析器，以确定如何将控制器返回的视图名称转换为实际的视图资源。
    default void configureViewResolvers(ViewResolverRegistry registry) {}
    // 用于定制 Spring MVC 如何处理 HTTP 请求和响应的消息转换器，包括 JSON、XML 等内容类型的转换
    default void configureMessageConverters(List<HttpMessageConverter<?>> converters) {}
    // 用于定制 Spring MVC 如何处理控制器方法中发生的异常，并提供相应的错误处理逻辑。
    default void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {}

    // 用于定制 Spring MVC 如何处理数据的格式化和解析，例如日期、数值等类型的对象的输入和输出格式。
    default void addFormatters(FormatterRegistry registry) {}
    // 用于定制 Spring MVC 如何使用拦截器来处理请求和响应，包括在请求进入控制器之前和之后执行特定的操作。
    default void addInterceptors(InterceptorRegistry registry) {}
    // 用于定制 Spring MVC 如何处理静态资源（如 CSS、JavaScript、图片等文件）的请求。
    default void addResourceHandlers(ResourceHandlerRegistry registry) {}
    // 用于定制 Spring MVC 如何处理跨域请求，确保应用程序可以正确地响应来自不同域名的 AJAX 请求或其他跨域请求。
    default void addCorsMappings(CorsRegistry registry) {}
    // 用于快速定义视图控制器，而无需编写完整的控制器类和方法。
    default void addViewControllers(ViewControllerRegistry registry) {}
    // 用于定制 Spring MVC 如何解析控制器方法中的参数，包括如何从请求中获取并转换参数值。
    default void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {}
    // 用于定制 Spring MVC 如何处理控制器方法的返回值，包括如何将返回值转换为实际的 HTTP 响应。
    default void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {}

    // 用于定制 Spring MVC 如何处理 HTTP 请求和响应的数据格式，允许你添加或调整默认的消息转换器，以支持特定的数据格式。
    default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {}
    // 用于定制 Spring MVC 的异常处理器，允许你添加额外的异常处理逻辑。
    default void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {}
}
```



#### `WebMvcConfigurer`接口的实现类`WebMvcAutoConfigurationAdapter`
`WebMvcAutoConfigurationAdapter`是Spring Boot框架提供的，实现了Spring MVC中的`WebMvcConfigurer`接口，对Spring MVC的所有行为进行了默认的配置。

如果想要改变这些默认配置，应该怎么办呢？看源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730353008999-076f3b3f-2a0b-4936-a9f2-5d6cfe88b346.png)

* **可以看到，该类上有一个注解`@EnableConfigurationProperties({ WebMvcProperties.class, WebProperties.class })`，该注解负责启用配置属性。会将配置文件`application.properties`或`application.yml`中的配置传递到该类中**。因此可以通过`application.properties`或`application.yml`配置文件来改变Spring Boot对SpringMVC的默认配置。`WebMvcProperties`和`WebProperties`源码如下： 

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730354494823-2adf83a4-aa2a-4fed-a5df-66b156de15c5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730354507221-6424a72a-0728-478b-92eb-e52d3d5a6f38.png)

通过以上源码得知要改变SpringBoot对SpringMVC的默认配置，需要在配置文件中使用以下前缀的配置：

+ **spring.mvc：主要用于配置 Spring MVC 的相关行为，例如路径匹配、视图解析、静态资源处理等**
+ **spring.web：通常用于配置一些通用的 Web 层设置，如资源处理、安全性配置等**。







## 6.自动配置中的静态资源处理
web站点中的静态资源指的是：js、css、图片等。

### ①静态资源处理源码分析
关于**SpringBoot对静态资源处理的默认配置**，查看`WebMvcAutoConfigurationAdapter`源码，核心源码如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730356581883-80169fed-c487-4b68-a1f2-70ef95d6c9a6.png)



对以上源码进行解释：
```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {

    // 检查 resourceProperties 中的 addMappings 属性是否为 false。如果为 false，则表示不启用默认的静态资源映射处理。该属性默认值是true
    // 在application.properties配置文件中进行`spring.web.resources.add-mappings=false`配置，可以将其设置为false。
    // 当然，如果没有配置的话，默认值是true。
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
    }

    // 配置 WebJars 的静态资源处理。webjars是现在前后端分离比较重要的一种静态资源打包方式
    // this.mvcProperties.getWebjarsPathPattern()的执行结果是：/webjars/**
    // 也就是说，如果请求路径是 http://localhost:8080/webjars/** ，则自动去类路径下的 /META-INF/resources/webjars/ 目录中找静态资源。
    // 如果要改变这个默认的配置，需要在application.properties文件中进行这样的配置：`spring.mvc.webjars-path-pattern=...`
    addResourceHandler(registry, this.mvcProperties.getWebjarsPathPattern(),
            "classpath:/META-INF/resources/webjars/");

    // 配置普通静态资源处理
    // this.mvcProperties.getStaticPathPattern()的执行结果是：/**
    // this.resourceProperties.getStaticLocations()的执行结果是：{ "classpath:/META-INF/resources/","classpath:/resources/", "classpath:/static/", "classpath:/public/" }
    // 也就是说，如果请求路径是：http://localhost:8080/**，根据控制器方法优先原则，会先去找合适的控制器方法，如果没有合适的控制器方法，静态资源处理才会生效，则自动去类路径下的/META-INF/resources/、/resources/、/static/、/public/ 4个位置找。
    // 如果要改变这个默认的配置，需要在application.properties中进行如下的两个配置：
    // 配置URL：spring.mvc.static-path-pattern=...
    // 配置物理路径：spring.web.resources.static-locations=...,...,...,...
    addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
        registration.addResourceLocations(this.resourceProperties.getStaticLocations());
        if (this.servletContext != null) {
            ServletContextResource resource = new ServletContextResource(this.servletContext, SERVLET_LOCATION);
            registration.addResourceLocations(resource);
        }
    });
}
```





### ②关于WebJars静态资源处理
**默认规则是：当请求路径是`/webjars/**`，则会去`classpath:/META-INF/resources/webjars/`找。**



**WebJars介绍**

**WebJars 是一种将常用的前端库（如 jQuery、Bootstrap、Font Awesome 等）打包成 JAR 文件的形式，方便在 Java 应用程序中使用**。WebJars 提供了一种标准化的方式来管理前端库，使其更容易集成到 Java 项目中，并且可以利用 Maven 的依赖管理功能。

<font style="color:rgb(44, 44, 54);"></font>

WebJars在SpringBoot中的使用**

WebJars官网：[https://www.webjars.org/](https://www.webjars.org/)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364165294-a000b7b4-9fdb-4c40-99f5-c47f2f25ad9e.png)




在官网上可以找到某个webjars的maven依赖，将依赖加入到SpringBoot项目中，例如我们添加vue的依赖：

```xml
<dependency>
    <groupId>org.webjars.npm</groupId>
    <artifactId>vue</artifactId>
    <version>3.5.12</version>
</dependency>
```

如下图表示加入成功：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364253405-dc6801f0-6122-49eb-9e77-36ef92668f5b.png)

在jar包列表中也可以看到：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364333436-43d60b44-fb7b-454d-b586-f948930ea146.png)







在SpringBoot中，对WebJars的默认访问规则是：当请求路径是`/webjars/**`，则会去`classpath:/META-INF/resources/webjars/`找。

因此我们要想访问上图的`index.js`，则应该发送这样的请求路径：`http://localhost:8080/webjars/vue/3.5.12/index.js`



启动服务器，打开浏览器，访问，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364535656-fd9fb574-ce9d-4278-96a4-7d71207e1446.png)





和IDEA中的文件对比一下，完全一样则表示测试成功：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364567084-7ac48323-e220-4e10-a4b6-dc5a30ab6955.png)




### ③关于普通静态资源处理
SpringBoot对普通静态资源处理的规则是：

当请求路径是[http://localhost:8080/**](http://localhost:8080/**)，根据控制器方法优先原则，会先去找合适的控制器方法，如果没有合适的控制器方法，静态资源处理才会生效，则自动去类路径下的以下4个位置查找：

+ classpath:/META-INF/resources/
+ classpath:/resources/
+ classpath:/static/
+ classpath:/public/ 

我们可以在项目中分别创建以上4个目录，在4个目录当中放入静态资源，例如4张图片：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364803886-d7b87794-d9b4-45ef-b5c2-3f2e4175675c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364994809-b6cfb675-1518-4a77-acbd-d8dfa5f4c638.png)



然后启动服务器，打开浏览器，访问，测试是否可以正常访问图片：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365066712-a89265e6-eb88-4306-b95b-bc85d2e72f9d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365080259-ad76485e-b65e-4660-9a70-8cfbc4497383.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365090833-de32f7cc-a61c-4896-b151-4851c8d10bc2.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365102733-3c375cf4-4bd2-4888-8090-b222a15907d3.png)



### ④关于静态资源缓存处理
不管是webjars的静态资源还是普通静态资源，统一都会执行以下这个方法，这个方法最后几行代码就是关于静态资源的缓存处理方式。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730362324912-ff074309-5a29-4ee9-84df-804311a07314.png)

什么是静态资源缓存，谁缓存，有什么用？

**静态资源缓存指的是浏览器的缓存行为，浏览器可以将静态资源（js、css、图片、声音、视频）缓存到浏览器中，只要下一次用户访问同样的静态资源直接从缓存中取**，不再从服务器中获取，可以降低服务器的压力，提高用户的体验。而**这个缓存策略可以在服务器端程序中进行设置，SpringBoot对静态资源缓存的默认策略就是以下这三行代码**：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365697951-98a6687d-dc95-49fe-bf63-6afac28ac62b.png)



以上三行代码的解释如下：

+ registration.setCachePeriod(getSeconds(this.resourceProperties.getCache().getPeriod()));
    - **设置缓存的过期时间（如果没有指定单位，默认单位是秒）**
    - 浏览器会**根据响应头中的缓存控制信息决定是否从本地缓存中加载资源，而不是每次都从服务器重新请求。这有助于减少网络流量和提高页面加载速度**。
    - 假设你配置了静态资源缓存过期时间为 1 小时（3600 秒），那么浏览器在首次请求某个静态资源后，会在接下来的一小时内从本地缓存加载该资源，而不是重新请求服务器。
    - 可以通过`application.properties`的来修改默认的过期时间，例如：spring.web.resources.cache.period=3600或者spring.web.resources.cache.period=1h`
+ registration.setCacheControl(this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl());
    - **设置静态资源的 Cache-Control HTTP 响应头，告诉浏览器如何去缓存这些资源**。
    - HTTP 响应头   是HTTP响应协议的一部分内容。如下图响应协议的响应头信息中即可看到Cache-Control的字样：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730367060571-fb49d8ba-39d5-4a04-9c6b-cbce0add9283.png)

	- 常见的 Cache-Control 指令包括：
		* max-age=\<seconds>：表示响应在多少秒内有效。
		* public：表示响应可以被任何缓存机制（如代理服务器）缓存。
		* private：表示响应只能被用户的浏览器缓存。
		* no-cache：表示在使用缓存的资源之前必须重新发送一次请求进行验证。
		* no-store：表示不缓存任何响应的资源。
	* 例如：max-age=3600, public：表示响应在 3600 秒内有效，并且可以被任何缓存机制缓存。
	* 可以通过`spring.web.resources.cache.cachecontrol.max-age=3600`以及`spring.web.resources.cache.cachecontrol.cache-public=true`进行重新配置。

+ registration.setUseLastModified(this.resourceProperties.getCache().isUseLastModified());
	- **设置静态资源在响应时，是否在响应头中添加资源的最后一次修改时间。SpringBoot默认配置的是：在响应头中添加响应资源的最后一次修改时间。**
	- 浏览器发送请求时，会将缓存中的资源的最后修改时间和服务器端资源的最后一次修改时间进行比对，如果没有变化，仍然从缓存中获取。
	- 可以通过`spring.web.resources.cache.use-last-modified=false`来进行重新配置。





### ⑤静态资源缓存测试
根据之前源码分析，得知`静态资源缓存`相关的配置应该使用`spring.web.resources.cache`：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429586708-40337969-1725-4459-879b-2299ab2a4405.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429704722-ad0a5409-038f-48f1-9986-809c24f82369.png)


![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429724705-e10888fe-f2aa-4b87-9581-e2641c3e30fd.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429747795-51fcc7b3-9dc6-49e3-8cbf-647cfff03b09.png)




在`application.properties`文件中对缓存进行如下的配置：

```properties
# 静态资源缓存设置
# 1. 缓存有效期
spring.web.resources.cache.period=100
# 2. 缓存控制（cachecontrol配置的话，period会失效）
spring.web.resources.cache.cachecontrol.max-age=20
# 3. 是否使用缓存的最后修改时间（默认是：使用）
spring.web.resources.cache.use-last-modified=true
# 4. 是否开启静态资源默认处理方式（默认是：开启）
spring.web.resources.add-mappings=true
```



注意：`cachecontrol.max-age`配置的话，`period`会被覆盖。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430084806-2086f0b6-646a-4e6a-a39c-8fa8dba74be0.png)






启动服务器测试：看看是否在20秒内走缓存，20秒之后是不是就不走缓存了！！！

第一次访问：请求服务器

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430573829-f3ac9ce5-fe18-4de9-85b8-4489c3799d74.png)

第二次访问：20秒内开启一个新的浏览器窗口，再次访问，发现走了缓存

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430633080-eb7ae6d1-fb6a-4bcd-beb4-e5df47d9fdf7.png)



第三次访问：20秒后开启一个新的浏览器窗口，再次访问，发现重新请求服务器

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430687390-dc5d00f4-9b08-43cd-9ffc-b538dd4f1fb6.png)

提示，为什么显示`304`，这是因为这个配置：`spring.web.resources.cache.use-last-modified=true`





### ⑥web应用的欢迎页面
#### 欢迎页测试
**先说结论：只要在静态资源路径下提供`index.html`，则被当做欢迎页面。静态资源路径指的是之前的4个路径：**

```plain
{ "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/" }
```

测试一下，在`classpath:/static/`目录下新建`index.html`页面：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422047114-993504b3-e8b2-4570-a789-53d1427bb0be.png)





启动服务器，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422096295-c5fedb9e-df71-4cac-9a24-f2d6da9b071f.png)

如果同时在4个静态资源路径下都提供`index.html`，哪个页面会被当做欢迎页呢？

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422239619-ba967027-498c-4f90-a139-c1565c91328d.png)

启动服务器，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422275754-3b1e145f-f34b-4eac-ae6f-0187eb852282.png)

原因是什么呢？这是因为`classpath:/META-INF/resources/`是数组的首元素，因此先从这个路径下找欢迎页。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422431137-93db3613-613e-434b-b7b8-c9f1af68636c.png)





#### 欢迎页源码分析
在`WebMvcAutoConfiguration`类中有一个内部类`EnableWebMvcConfiguration`，这个类中有这样一段代码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424626175-06c18960-d1bc-4ea7-a9f2-84079dd105ca.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424702185-b359161c-2221-4106-837c-1a6a4bb16683.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424718260-3d97f911-f050-42a6-b2c3-78a739d39f2d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424745404-720c4fe8-118e-4133-9514-efab29e7e39e.png)

通过以上源码追踪，得出结论：只要请求路径是`/**`的，会依次去`{ "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/" }`这四个位置找`index.html`页面作为欢迎页。





#### 一个小小的疑惑
我们来看一下`WebMvcAutoConfiguration`的生效条件：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424986145-0d76b7da-06c5-4942-a5f2-3e55f0249c89.png)

上图红框内表示，要求Spring容器中缺失`WebMvcConfigurationSupport`这个Bean，`WebMvcAutoConfiguration`才会生效。



但是我们来看一下`EnableWebMvcConfiguration`的继承结构：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730425120392-dda2febe-d932-452c-9599-6df5859a2062.png)

很明显，`EnableWebMvcConfiguration`就是一个`WebMvcConfigurationSupport`这样的Bean。

那疑问就有了：既然容器中存在`WebMvcConfigurationSupport`这样的Bean，`WebMvcAutoConfiguration`为什么还会生效呢？

原因是因为：`EnableWebMvcConfiguration`是`WebMvcAutoConfiguration`类的内部类。在`WebMvcAutoConfiguration`进行加载的时候，`EnableWebMvcConfiguration`这个内部类还没有加载。因此这个时候在容器中还不存在`WebMvcConfigurationSupport`的Bean，所以`WebMvcAutoConfiguration`仍然会生效。



**<font style="color:#DF2A3F;">以上所说的</font>**`**<font style="color:#DF2A3F;">WebMvcAutoConfiguration</font>**`**<font style="color:#DF2A3F;">类中的内部类</font>**`**<font style="color:#DF2A3F;">EnableWebMvcConfiguration</font>**`**<font style="color:#DF2A3F;">，是用来启用Web MVC默认配置的。</font>**

**<font style="color:#DF2A3F;"></font>**

**<font style="color:#DF2A3F;">注意区分：WebMvcAutoConfiguration的两个内部类：</font>**

+ `WebMvcAutoConfigurationAdapter`作用是用来：修改配置的
+ `EnableWebMvcConfiguration`作用是用来：启用配置的

#### favorite icon
**favicon（也称为“收藏夹图标”或“网站图标”）是大多数现代网页浏览器的默认行为之一。当用户访问一个网站时，浏览器通常会尝试从该网站的根目录下载名为 favicon.ico 的文件，并将其用作标签页的图标。**

如果网站没有提供 favicon.ico 文件，浏览器可能会显示一个默认图标，或者根本不显示任何图标。为了确保良好的用户体验，网站开发者通常会在网站的根目录下放置一个 favicon.ico 文件。



Spring Boot项目中`favicon.ico`文件应该放在哪里呢？Spring Boot官方是这样说明的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730427556830-9ba1ca93-5b91-477c-af0f-af51ee850b31.png)

这段话翻译为：

**与其他静态资源一样，Spring Boot 会在配置的静态内容位置检查是否存在 `favicon.ico`文件。如果存在这样的文件，它将自动作为应用程序的 favicon 使用。**



以上官方说明的：将`favicon.ico`文件放到静态资源路径下即可。



web站点没有提供`favicon.ico`时：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730427725460-6c4062dc-df53-495c-8709-431dd38f2337.png)



我们在[https://www.iconfont.cn/](https://www.iconfont.cn/) （阿里巴巴提供的图标库）上随便找一个图标，然后将图片名字命名为`favicon.ico`，然后将其放到SpringBoot项目的静态资源路径下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730427919469-60975be4-d0d3-43ba-8435-98d9da427282.png)

启动服务器测试：记住（ctrl + F5强行刷新一下，避免影响测试效果）

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730428251364-894a09fe-92fb-408c-89d0-42d8e27b222b.png)




