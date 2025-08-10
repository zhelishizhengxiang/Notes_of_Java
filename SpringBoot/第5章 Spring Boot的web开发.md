![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# SpringBoot的web自动配置
新建项目`sb3-09-web`：添加web启动器，添加Lombok依赖。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729842528256-345582ac-9fbe-464f-9d1a-3ee7800bea8a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## web自动配置的依赖是如何传递的
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

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

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

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

4. 在`spring-boot-autoconfigure`包中的`.imports`文件中罗列的需要导入的自动配置类，如下图：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729843518728-be8a441f-d6cc-4781-8f0e-a3c67a05b697.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## web自动配置的实现原理
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

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

入口程序被`@SpringBootApplication`注解标注。

2. `@SpringBootApplication`注解被`@EnableAutoConfiguration`<font style="color:#080808;background-color:#ffffff;">注解标注。表示启用自动配置。</font>
3. `@EnableAutoConfiguration`<font style="color:#080808;background-color:#ffffff;">注解被</font>`@Import({AutoConfigurationImportSelector.class})`<font style="color:#080808;background-color:#ffffff;">注解标注。</font>
4. <font style="color:#080808;background-color:#ffffff;">因此</font>`AutoConfigurationImportSelector`<font style="color:#080808;background-color:#ffffff;">决定哪些自动配置类是需要导入的。</font>
5. `<font style="color:#080808;background-color:#ffffff;">AutoConfigurationImportSelector</font>`<font style="color:#080808;background-color:#ffffff;">底层实现步骤具体如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845866175-609a5c89-9342-4d96-b624-fcc5a7dc2434.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845887890-372b80d6-2e7c-4d7e-8c5c-40cc3402da91.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845909271-f6640224-ae46-4d74-bea2-ff00fa4e95d7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729845939380-9158c6fe-9ac2-43e3-8b4a-fb9b568eefd3.png)

最终找的文件是：`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`

**注意：任何jar包，包括第三方的依赖，自动配置类所在的路径以及文件名都是完全相同的，都是**`**META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports**`

例如mybatis的自动配置类的列表文件也是这样，如下图：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729846670846-84850d75-8a0f-418c-8c64-f48f00f3e738.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 通过web自动配置类逆推web配置的prefix
在自动配置列表中找到web自动配置相关的类：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729846834747-246f3a88-f822-46bc-a00c-7770d52c5c26.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



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



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



通过web自动配置类的源码可以逆推web配置的prefix：

1. WebMvcAutoConfiguration

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847170017-0f711f67-f266-4ac2-a25a-c239475b38fe.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847235858-7cab9d78-465f-4289-be9c-9b399570e8d3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847268539-715a377a-ecb9-48fc-ad58-9194d23b7cdf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

2. MultipartAutoConfiguration

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847417032-2abad318-a48b-49a7-a2dc-26abd0f4d8e5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847475254-4f486638-6132-40c0-baee-25c4c3091f49.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

3. HttpEncodingAutoConfiguration

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847580938-8114e69c-ba29-4305-ba80-3b0d277be2e1.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729847644668-2d8aa37a-699d-4009-92c7-3c9a60efcb49.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

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

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## Web自动配置都默认配置了什么
查看官方文档：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731383415638-90d102e9-1fd0-4f31-9736-9e3bef3936d3.png)



**翻译如下：**

Spring Boot 为 Spring MVC 提供了自动配置，这在大多数应用程序中都能很好地工作。除了已经实现了 Spring MVC 的默认功能外，自动配置还提供了以下特性：

+ 包括 `ContentNegotiatingViewResolver` 和 `BeanNameViewResolver` 的 Bean。
    - `ContentNegotiatingViewResolver` 自动根据HTTP请求头中Accept字段来选择合适的视图技术渲染响应。
    - `<font style="color:rgb(44, 44, 54);">BeanNameViewResolver</font>`<font style="color:rgb(44, 44, 54);"> 的作用是根据视图名称找到视图View对象。</font>
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

如果您不想使用自动配置并希望完全控制 Spring MVC，可以添加您自己的带有 `**<font style="color:#DF2A3F;">@EnableWebMvc</font>**` 注解的 `@Configuration`。

如果您希望保留这些 Spring Boot MVC 定制化设置并进行更多的 MVC 定制化（如拦截器、格式化程序、视图控制器等其他功能），可以添加您自己的类型为 `WebMvcConfigurer` 的 `@Configuration` 类。**<font style="color:#DF2A3F;">但不能使用</font>**`**<font style="color:#DF2A3F;">@EnableWebMvc</font>**`**<font style="color:#DF2A3F;">注解</font>**。





![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## <font style="color:#080808;background-color:#ffffff;">WebMvcAutoConfiguration原理</font>
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
    - **WebMvcAutoConfiguration自动配置类**加载顺序在以上自动配置类加载后加载。
+ @ConditionalOnWebApplication(type = Type.SERVLET)
    - **WebMvcAutoConfiguration自动配置类**只在servlet环境中生效。
+ @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
    - 类路径中必须存在`Servlet.class``DispatcherServlet.class``WebMvcConfigurer.class`，**WebMvcAutoConfiguration自动配置类才会生效。**
+ @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
    - 类路径中不存在`WebMvcConfigurationSupport.class`时**WebMvcAutoConfiguration自动配置类才会生效。**
    - 注意：当使用@EnableWebMvc注解后，类路径中就会注册一个WebMvcConfigurationSupport这样的bean。
+ @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10) **不重要**
    - 指定**WebMvcAutoConfiguration自动配置类**的加载顺序
+ @ImportRuntimeHints(WebResourcesRuntimeHints.class) **不重要**
    - 运行时引入**WebResourcesRuntimeHints**这个类，这个类的作用是给JVM或者其他组件提示信息的，提示一下系统应该如何处理类和资源。



总结来说，WebMvcAutoConfiguration类将在以下条件下生效：

1. 应用程序是一个Servlet类型的Web应用；
2. 环境中有Servlet、DispatcherServlet和WebMvcConfigurer类；
3. 容器中没有WebMvcConfigurationSupport的bean。

如果这些条件都满足的话，那么这个自动配置类就会被激活，并进行相应的自动配置工作。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### WebMvc自动配置生效后引入了两个Filter Bean
#### 引入了<font style="color:#080808;background-color:#ffffff;">HiddenHttpMethodFilter Bean</font>
```java
@Bean
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled")
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new OrderedHiddenHttpMethodFilter();
}
```

这个过滤器是专门处理Rest请求的。GET POST PUT DELETE请求。

#### 引入了<font style="color:#080808;background-color:#ffffff;">FormContentFilter Bean</font>
```java
@Bean
@ConditionalOnMissingBean(FormContentFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.formcontent.filter", name = "enabled", matchIfMissing = true)
public OrderedFormContentFilter formContentFilter() {
    return new OrderedFormContentFilter();
}
```

OrderedFormContentFilter 是 Spring Boot 中用于处理 HTTP 请求的一个过滤器，特别是针对 PUT 和 DELETE 请求。这个过滤器的主要作用是在处理 PUT 和 DELETE 请求时，确保如果请求体中有表单格式的数据，这些数据会被正确解析并可用。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### WebMvc自动配置生效后引入了WebMvcConfigurer接口的实现类
在SpringBoot框架的`WebMvcAutoConfiguration`类中提供了一个内部类：`WebMvcAutoConfigurationAdapter`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730345630650-62e11666-3e95-4c64-9787-c0b18d083c91.png)

SpringBoot在这个类`WebMvcAutoConfigurationAdapter`中进行了一系列的Spring MVC相关配置。

**<font style="color:#DF2A3F;">我们开发中要对Spring MVC的相关配置进行修改，可以编写一个类继承</font>**`**<font style="color:#DF2A3F;">WebMvcAutoConfigurationAdatper</font>**`**<font style="color:#DF2A3F;">，然后重写对应的方法即可。</font>**

**<font style="color:#DF2A3F;">因此，通过对</font>**`**<font style="color:#DF2A3F;">WebMvcAutoConfigurationAdapter</font>**`**<font style="color:#DF2A3F;">类中的方法进行重写来</font>****<font style="color:#74B602;">修改</font>****<font style="color:#DF2A3F;">Web MVC的默认配置。</font>**

**<font style="color:#DF2A3F;"></font>**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

#### 关于`WebMvcConfigurer`接口
这个接口不是SpringBoot框架提供的，是Spring MVC提供的，在Spring框架4.3版本中引入的。这个接口的作用主要是**允许开发者通过实现这个接口来定制Spring MVC的行为**。

在这个接口中提供了很多方法，需要改变Spring MVC的哪个行为，则重写对应的方法即可，下面是这个接口中所有的方法，以及每个方法对应的Spring MVC行为的解释：

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
    // 用于定制 Spring MVC 解析视图的方式，以确定如何将控制器返回的视图名称转换为实际的视图资源。
    default void configureViewResolvers(ViewResolverRegistry registry) {}
    // 用于定制 Spring MVC 如何处理 HTTP 请求和响应的数据格式，包括 JSON、XML 等内容类型的转换
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
    // 用于快速定义简单的 URL 到视图的映射，而无需编写完整的控制器类和方法。
    default void addViewControllers(ViewControllerRegistry registry) {}
    // 用于定制 Spring MVC 如何解析控制器方法中的参数，包括如何从请求中获取并转换参数值。
    default void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {}
    // 用于定制 Spring MVC 如何处理控制器方法的返回值，包括如何将返回值转换为实际的 HTTP 响应。
    default void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {}

    // 用于定制 Spring MVC 如何处理 HTTP 请求和响应的数据格式，允许你添加或调整默认的消息转换器，以支持特定的数据格式。
    default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {}
    // 用于定制 Spring MVC 如何处理控制器方法中抛出的异常，允许你添加额外的异常处理逻辑。
    default void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {}
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

#### `WebMvcConfigurer`接口的实现类`WebMvcAutoConfigurationAdapter`
`WebMvcAutoConfigurationAdapter`是Spring Boot框架提供的，实现了Spring MVC中的`WebMvcConfigurer`接口，对Spring MVC的所有行为进行了默认的配置。

如果想要改变这些默认配置，应该怎么办呢？看源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730353008999-076f3b3f-2a0b-4936-a9f2-5d6cfe88b346.png)

可以看到，该类上有一个注解`@EnableConfigurationProperties({ WebMvcProperties.class, WebProperties.class })`，该注解负责启用配置属性。会将配置文件`application.properties`或`application.yml`中的配置传递到该类中。因此可以通过`application.properties`或`application.yml`配置文件来改变Spring Boot对SpringMVC的默认配置。`WebMvcProperties`和`WebProperties`源码如下： 

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730354494823-2adf83a4-aa2a-4fed-a5df-66b156de15c5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730354507221-6424a72a-0728-478b-92eb-e52d3d5a6f38.png)

通过以上源码得知要改变SpringBoot对SpringMVC的默认配置，需要在配置文件中使用以下前缀的配置：

+ spring.mvc：<font style="color:rgb(44, 44, 54);">主要用于配置 Spring MVC 的相关行为，例如路径匹配、视图解析、静态资源处理等</font>
+ spring.web：通常用于配置一些通用的 Web 层设置，如资源处理、安全性配置等。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



## 自动配置中的静态资源处理
web站点中的静态资源指的是：js、css、图片等。

### 静态资源处理源码分析
关于**SpringBoot对静态资源处理的默认配置**，查看`WebMvcAutoConfigurationAdapter`源码，核心源码如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730356581883-80169fed-c487-4b68-a1f2-70ef95d6c9a6.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

对以上源码进行解释：

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {

    // 检查 resourceProperties 中的 addMappings 属性是否为 false。如果为 false，则表示不启用默认的静态资源映射处理。
    // 在application.properties配置文件中进行`spring.web.resources.add-mappings=false`配置，可以将其设置为false。
    // 当然，如果没有配置的话，默认值是true。
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
    }

    // 配置 WebJars 的静态资源处理。
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



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 关于WebJars静态资源处理
默认规则是：当请求路径是`/webjars/**`，则会去`classpath:/META-INF/resources/webjars/`找。



**WebJars介绍**

<font style="color:rgb(44, 44, 54);">WebJars 是一种将常用的前端库（如 jQuery、Bootstrap、Font Awesome 等）打包成 JAR 文件的形式，方便在 Java 应用程序中使用。WebJars 提供了一种标准化的方式来管理前端库，使其更容易集成到 Java 项目中，并且可以利用 Maven 的依赖管理功能。</font>

<font style="color:rgb(44, 44, 54);"></font>

**<font style="color:rgb(44, 44, 54);">WebJars在SpringBoot中的使用</font>**

<font style="color:rgb(44, 44, 54);">WebJars官网：</font>[https://www.webjars.org/](https://www.webjars.org/)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364165294-a000b7b4-9fdb-4c40-99f5-c47f2f25ad9e.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

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



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



在SpringBoot中，对WebJars的默认访问规则是：当请求路径是`/webjars/**`，则会去`classpath:/META-INF/resources/webjars/`找。

因此我们要想访问上图的`index.js`，则应该发送这样的请求路径：`http://localhost:8080/webjars/vue/3.5.12/index.js`



启动服务器，打开浏览器，访问，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364535656-fd9fb574-ce9d-4278-96a4-7d71207e1446.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

和IDEA中的文件对比一下，完全一样则表示测试成功：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364567084-7ac48323-e220-4e10-a4b6-dc5a30ab6955.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 关于普通静态资源处理
SpringBoot对普通静态资源处理的规则是：

当请求路径是[http://localhost:8080/**](http://localhost:8080/**)，根据控制器方法优先原则，会先去找合适的控制器方法，如果没有合适的控制器方法，静态资源处理才会生效，则自动去类路径下的以下4个位置查找：

+ classpath:/META-INF/resources/
+ classpath:/resources/
+ classpath:/static/
+ classpath:/public/ 

我们可以在项目中分别创建以上4个目录，在4个目录当中放入静态资源，例如4张图片：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364803886-d7b87794-d9b4-45ef-b5c2-3f2e4175675c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730364994809-b6cfb675-1518-4a77-acbd-d8dfa5f4c638.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

然后启动服务器，打开浏览器，访问，测试是否可以正常访问图片：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365066712-a89265e6-eb88-4306-b95b-bc85d2e72f9d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365080259-ad76485e-b65e-4660-9a70-8cfbc4497383.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365090833-de32f7cc-a61c-4896-b151-4851c8d10bc2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365102733-3c375cf4-4bd2-4888-8090-b222a15907d3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 关于静态资源缓存处理
不管是webjars的静态资源还是普通静态资源，统一都会执行以下这个方法，这个方法最后几行代码就是关于静态资源的缓存处理方式。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730362324912-ff074309-5a29-4ee9-84df-804311a07314.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**什么是静态资源缓存，谁缓存，有什么用？**

静态资源缓存指的是浏览器的缓存行为，浏览器可以将静态资源（js、css、图片、声音、视频）缓存到浏览器中，只要下一次用户访问同样的静态资源直接从缓存中取，不再从服务器中获取，可以降低服务器的压力，提高用户的体验。而这个缓存策略可以在服务器端程序中进行设置，SpringBoot对静态资源缓存的默认策略就是以下这三行代码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730365697951-98a6687d-dc95-49fe-bf63-6afac28ac62b.png)



以上三行代码的解释如下：

+ registration.setCachePeriod(getSeconds(this.resourceProperties.getCache().getPeriod()));
    - 设置缓存的过期时间（如果没有指定单位，默认单位是秒）
    - 浏览器会**根据响应头中的缓存控制信息**决定是否从本地缓存中加载资源，而不是每次都从服务器重新请求。这有助于减少网络流量和提高页面加载速度。
    - 假设你配置了静态资源缓存过期时间为 1 小时（3600 秒），那么浏览器在首次请求某个静态资源后，会在接下来的一小时内从本地缓存加载该资源，而不是重新请求服务器。
    - 可以通过`application.properties`的来修改默认的过期时间，例如：`<font style="color:rgb(51, 51, 51);">spring.web.resources.cache.period=3600</font>`<font style="color:rgb(51, 51, 51);">或者</font>`<font style="color:rgb(51, 51, 51);">spring.web.resources.cache.period=1h</font>`
+ registration.setCacheControl(this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl());
    - 设置静态资源的 Cache-Control HTTP 响应头，告诉浏览器如何去缓存这些资源。
    - `<font style="color:rgb(44, 44, 54);">Cache-Control</font>`<font style="color:rgb(44, 44, 54);"> HTTP 响应头   是HTTP响应协议的一部分内容。如下图响应协议的响应头信息中即可看到</font>`<font style="color:rgb(44, 44, 54);">Cache-Control</font>`<font style="color:rgb(44, 44, 54);">的字样：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730367060571-fb49d8ba-39d5-4a04-9c6b-cbce0add9283.png)

    - <font style="color:rgb(44, 44, 54);">常见的 Cache-Control 指令包括：</font>
        * <font style="color:rgb(44, 44, 54);">max-age=<seconds>：表示响应在多少秒内有效。</font>
        * <font style="color:rgb(44, 44, 54);">public：表示响应可以被任何缓存机制（如代理服务器）缓存。</font>
        * <font style="color:rgb(44, 44, 54);">private：表示响应只能被用户的浏览器缓存。</font>
        * <font style="color:rgb(44, 44, 54);">no-cache：表示在使用缓存的资源之前必须重新发送一次请求进行验证。</font>
        * <font style="color:rgb(44, 44, 54);">no-store：表示不缓存任何响应的资源。</font>
    - 例如：max-age=3600, public：表示响应在 3600 秒内有效，并且可以被任何缓存机制缓存。
    - 可以通过`spring.web.resources.cache.cachecontrol.max-age=3600`以及`spring.web.resources.cache.cachecontrol.cache-public=true`进行重新配置。
+ registration.setUseLastModified(this.resourceProperties.getCache().isUseLastModified());
    - 设置静态资源在响应时，是否在响应头中添加资源的最后一次修改时间。SpringBoot默认配置的是：在响应头中添加响应资源的最后一次修改时间。
    - 浏览器发送请求时，会将缓存中的资源的最后修改时间和服务器端资源的最后一次修改时间进行比对，如果没有变化，仍然从缓存中获取。
    - 可以通过`spring.web.resources.cache.use-last-modified=false`来进行重新配置。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 静态资源缓存测试
根据之前源码分析，得知`静态资源缓存`相关的配置应该使用`spring.web.resources.cache`：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429586708-40337969-1725-4459-879b-2299ab2a4405.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429704722-ad0a5409-038f-48f1-9986-809c24f82369.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429724705-e10888fe-f2aa-4b87-9581-e2641c3e30fd.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730429747795-51fcc7b3-9dc6-49e3-8cbf-647cfff03b09.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



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



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



启动服务器测试：看看是否在20秒内走缓存，20秒之后是不是就不走缓存了！！！

第一次访问：请求服务器

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430573829-f3ac9ce5-fe18-4de9-85b8-4489c3799d74.png)

第二次访问：20秒内开启一个新的浏览器窗口，再次访问，发现走了缓存

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430633080-eb7ae6d1-fb6a-4bcd-beb4-e5df47d9fdf7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

第三次访问：20秒后开启一个新的浏览器窗口，再次访问，发现重新请求服务器

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730430687390-dc5d00f4-9b08-43cd-9ffc-b538dd4f1fb6.png)

提示，为什么显示`304`，这是因为这个配置：`spring.web.resources.cache.use-last-modified=true`



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### web应用的欢迎页面
#### 欢迎页测试
先说结论：只要在静态资源路径下提供`index.html`，则被当做欢迎页面。静态资源路径指的是之前的4个路径：

```plain
{ "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/" }
```

测试一下，在`classpath:/static/`目录下新建`index.html`页面：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422047114-993504b3-e8b2-4570-a789-53d1427bb0be.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

启动服务器，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422096295-c5fedb9e-df71-4cac-9a24-f2d6da9b071f.png)

如果同时在4个静态资源路径下都提供`index.html`，哪个页面会被当做欢迎页呢？

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422239619-ba967027-498c-4f90-a139-c1565c91328d.png)

启动服务器，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422275754-3b1e145f-f34b-4eac-ae6f-0187eb852282.png)

原因是什么呢？这是因为`classpath:/META-INF/resources/`是数组的首元素，因此先从这个路径下找欢迎页。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730422431137-93db3613-613e-434b-b7b8-c9f1af68636c.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

#### 欢迎页源码分析
在`WebMvcAutoConfiguration`类中有一个内部类`EnableWebMvcConfiguration`，这个类中有这样一段代码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424626175-06c18960-d1bc-4ea7-a9f2-84079dd105ca.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424702185-b359161c-2221-4106-837c-1a6a4bb16683.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424718260-3d97f911-f050-42a6-b2c3-78a739d39f2d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424745404-720c4fe8-118e-4133-9514-efab29e7e39e.png)

通过以上源码追踪，得出结论：只要请求路径是`/**`的，会依次去`{ "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/" }`这四个位置找`index.html`页面作为欢迎页。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

#### 一个小小的疑惑
我们来看一下`WebMvcAutoConfiguration`的生效条件：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730424986145-0d76b7da-06c5-4942-a5f2-3e55f0249c89.png)

上图红框内表示，要求Spring容器中缺失`WebMvcConfigurationSupport`这个Bean，`WebMvcAutoConfiguration`才会生效。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

但是我们来看一下`EnableWebMvcConfiguration`的继承结构：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730425120392-dda2febe-d932-452c-9599-6df5859a2062.png)

很明显，`EnableWebMvcConfiguration`就是一个`WebMvcConfigurationSupport`这样的Bean。

那疑问就有了：既然容器中存在`WebMvcConfigurationSupport`这样的Bean，`WebMvcAutoConfiguration`为什么还会生效呢？

原因是因为：`EnableWebMvcConfiguration`是`WebMvcAutoConfiguration`类的内部类。在`WebMvcAutoConfiguration`进行加载的时候，`EnableWebMvcConfiguration`这个内部类还没有加载。因此这个时候在容器中还不存在`WebMvcConfigurationSupport`的Bean，所以`WebMvcAutoConfiguration`仍然会生效。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**<font style="color:#DF2A3F;">以上所说的</font>**`**<font style="color:#DF2A3F;">WebMvcAutoConfiguration</font>**`**<font style="color:#DF2A3F;">类中的内部类</font>**`**<font style="color:#DF2A3F;">EnableWebMvcConfiguration</font>**`**<font style="color:#DF2A3F;">，是用来启用Web MVC默认配置的。</font>**

**<font style="color:#DF2A3F;"></font>**

**<font style="color:#DF2A3F;">注意区分：WebMvcAutoConfiguration的两个内部类：</font>**

+ `WebMvcAutoConfigurationAdapter`作用是用来：修改配置的
+ `EnableWebMvcConfiguration`作用是用来：启用配置的

#### favorite icon
favicon（也称为“收藏夹图标”或“网站图标”）是大多数现代网页浏览器的默认行为之一。当用户访问一个网站时，浏览器通常会尝试从该网站的根目录下载名为 favicon.ico 的文件，并将其用作标签页的图标。

如果网站没有提供 favicon.ico 文件，浏览器可能会显示一个默认图标，或者根本不显示任何图标。为了确保良好的用户体验，网站开发者通常会在网站的根目录下放置一个 favicon.ico 文件。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

Spring Boot项目中`favicon.ico`文件应该放在哪里呢？Spring Boot官方是这样说明的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730427556830-9ba1ca93-5b91-477c-af0f-af51ee850b31.png)

这段话翻译为：

与其他静态资源一样，Spring Boot 会在配置的静态内容位置检查是否存在 `favicon.ico`文件。如果存在这样的文件，它将自动作为应用程序的 favicon 使用。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

以上官方说明的：将`favicon.ico`文件放到静态资源路径下即可。



web站点没有提供`favicon.ico`时：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730427725460-6c4062dc-df53-495c-8709-431dd38f2337.png)



我们在[https://www.iconfont.cn/](https://www.iconfont.cn/) （阿里巴巴提供的图标库）上随便找一个图标，然后将图片名字命名为`favicon.ico`，然后将其放到SpringBoot项目的静态资源路径下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730427919469-60975be4-d0d3-43ba-8435-98d9da427282.png)

启动服务器测试：记住（ctrl + F5强行刷新一下，避免影响测试效果）

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730428251364-894a09fe-92fb-408c-89d0-42d8e27b222b.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# SpringBoot的web手动配置(静态资源处理)
如果你对SpringBoot默认的静态资源处理方式不满意。可以通过两种方式来改变这些默认的配置：

+ 第一种：配置文件方式
    - 通过修改`application.properties`或`application.yml`
    - 添加`spring.mvc`和`spring.web`相关的配置。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730431752471-f806b5a3-924c-4b84-9c13-6999dad0493b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730431765824-5d3ec11d-aad5-44c8-a32e-dd255bec24ea.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730431775131-4bec5477-fd86-4ab2-afa1-4b1927b2aafc.png)

+ 第二种：编写代码方式
    - SpringMVC框架为我们提供了`WebMvcConfigurer`接口，需要改变默认的行为，可以`编写一个类`实现`WebMvcConfigurer`接口，并`对应重写`接口中的方法即可改变默认的配置行为。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 配置文件方式
要修改`访问静态资源URL的前缀`，这样配置：

```properties
# Spring MVC的相关配置
# 1. 设置webjars静态资源的请求路径的前缀
spring.mvc.webjars-path-pattern=/wjs/**
# 2. 设置普通静态资源的请求路径的前缀
spring.mvc.static-path-pattern=/static/**
```

要修改`静态资源的存放位置`，这样配置：

```properties
spring.web.resources.static-locations=classpath:/static1/,classpath:/static2/
```

进行以上配置之后：

1. 访问webjars的请求路径应该是这样的：http://localhost:8080/wjs/....
2. 访问普通静态资源的请求路径应该是这样的：http://localhost:8080/static/....
3. 普通静态资源的存放位置也应该放到`classpath:/static1/,classpath:/static2/`下面，其他位置无效。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**访问webjars测试结果如下：**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433063487-fe214e35-a43c-4ad0-9760-742f6bfcdcf6.png)



**访问普通静态资源测试结果如下：**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433000967-37e76b55-5715-4387-a135-5daacd53a755.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433110267-b2f84e91-6b95-48d3-88d6-acff3dca26cc.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433134598-5ac4583f-e7c1-4a8e-a3bb-77e2a3a33ac7.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

如果访问`dog2.jpg`，就无法访问了：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433508569-ce398096-8cae-42e3-b6a2-5fe94e2a007a.png)



但是，存储在`classpath:/META-INF/resources/`目录下的`dog1.jpg`仍然是可以访问的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433537233-e7fe1330-22d3-4b36-8c43-bbdc9a578f99.png)

因此，存储在`classpath:/META-INF/resources/`位置的静态资源会被默认加载，不受手动配置的影响。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 编写代码方式
我们在前面提到过，想要定制Spring MVC的行为，也可以编写类实现Spring MVC框架提供的一个接口`WebMvcConfigurer`，想定制哪个行为就重写哪个方法即可。

编写的类只要纳入IoC容器的管理。因此有以下两种实现方式：

+ 第一种：编写类实现`WebMvcConfigurer`接口，重写对应的方法。
+ 第二种：以组件的形式存在：编写一个方法，用`@Bean`注解标注。

### 第一种方式
编写配置类，对于`web`开发来说，配置类一般起名为：`WebConfig`。配置类一般存放到`config`包下，因此在SpringBoot主入口程序同级目录下新建`config`包，在`config`包下新建`WebConfig`类：

```java
package com.powernode.springboot.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

// 使用该注解标注，表示该类为配置类。
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
                .addResourceLocations("classpath:/static1/", "classpath:/static2/");
    }
}
```

注意：将`application.properties`文件中之前的所有配置全部注释掉。让其恢复到最原始的默认配置。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444156893-d646065e-96dc-449c-a8cc-eff70e23594c.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

启动服务器进行测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444208880-ca582b52-a68f-4918-9ead-f342c5ebbf38.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444226844-d1e40613-8c6c-43a5-95a1-1c0d34d101bb.png)

通过测试，我们的配置是生效的。

我们再来看看，默认的配置是否还生效？

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444289901-3304e315-67e2-4d6c-93b8-0db5f48a24a7.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

我们可以看到，Spring Boot对Spring MVC的默认自动配置是生效的。

**<font style="color:#DF2A3F;">因此，以上的方式只是在Spring MVC默认行为之外扩展行为。</font>**

**<font style="color:#DF2A3F;"></font>**

如果你不想再继续使用SpringBoot提供的默认行为，可以使用`@EnableWebMvc`进行标注。例如：

```java
package com.powernode.springboot.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@EnableWebMvc
// 使用该注解标注，表示该类为配置类。
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
                .addResourceLocations("classpath:/static1/", "classpath:/static2/");
    }
}

```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444605358-87fe9bc0-edfb-4fff-9bc0-ae4a97b50042.png)

可以看到，默认配置已经不再生效。



再来看看，我们自己的配置是否仍然生效：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444650423-57b49f40-b595-4c27-935e-1c6361f20ba4.png)

仍然生效。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 第二种方式
采用`@Bean`注解提供一个`WebMvcConfigurer`组件，代码如下：

```java
package com.powernode.springboot.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig2 {

    @Bean
    public WebMvcConfigurer addResourceHandlers(){
        return new WebMvcConfigurer() {
            @Override
            public void addResourceHandlers(ResourceHandlerRegistry registry) {
                registry.addResourceHandler("/static/**")
                        .addResourceLocations("classpath:/static1/", "classpath:/static2/");
            }
        };
    }
}

```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444971338-0aca22a6-ff00-4644-9e21-2d08765b62e4.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444986018-554aeccd-a0fd-43d2-9b92-7846fea2415c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730445003557-89783ce1-fdf7-4b22-873b-0fafdc07ecd2.png)

通过了测试，并且以上代码也是在原有配置基础上进行扩展。



如果要弃用默认配置，仍然使用	`@EnableWebMvc`注解进行标注。自行测试！



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 其他配置也这样做即可
以上对`静态资源处理`进行了手动配置，也可以做其他配置，例如拦截器：

```java
package com.powernode.springboot.config;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig2 {

    @Bean
    public WebMvcConfigurer addResourceHandlers(){
        return new WebMvcConfigurer() {
            @Override
            public void addResourceHandlers(ResourceHandlerRegistry registry) {
                registry.addResourceHandler("/static/**")
                        .addResourceLocations("classpath:/static1/", "classpath:/static2/");
            }
        };
    }

    // 拦截器配置。
    @Bean
    public WebMvcConfigurer addInterceptor(){
        return new WebMvcConfigurer() {
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                registry.addInterceptor(new HandlerInterceptor() {
                    @Override
                    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
                        System.out.println("Interceptor's preHandle......");
                        return true;
                    }

                    @Override
                    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
                        System.out.println("Interceptor's postHandle......");
                    }

                    @Override
                    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
                        System.out.println("Interceptor's afterCompletion......");
                    }
                });
            }
        };
    }
}

```

启动服务器，打开浏览器，发送请求[http://localhost:8080/static/dog5.jpg](http://localhost:8080/static/dog5.jpg)，后台执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730445490551-7c8fbe3f-a792-4307-8c46-21a365e0b25d.png)

这说明拦截器生效。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 为什么只要容器中有`WebMvcConfigurer`组件即可呢
源码分析：



`WebMvcAutoConfiguration`部分源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730446108216-7ff9fb75-c9dd-4ba3-8b59-23f6c78d0b1a.png)

`WebMvcAutoConfiguration`类的内部类`EnableWebMvcConfiguration`，这个类继承了`DelegatingWebMvcConfiguration`（Delegating是委派的意思。）

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

`DelegatingWebMvcConfiguration`部分源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730446241532-6f81bf0c-4a9b-43a1-b816-12ccb6cfdeeb.png)

`DelegatingWebMvcConfiguration`中的`setConfigurers()`方法用来设置配置。而配置参数是一个List集合，这个List集合中存放的是`WebMvcConfigurer`接口的实例，并且可以看到这个方法上面使用了`@Autowired`进行了自动注入，这也就是说为什么只要是IoC容器中的组件就能生效的原因。

我们再次进入到`this.configurers.addWebMvcConfigurers(configurers);`方法中进一步查看源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730446847673-e2758f97-fcc9-4c72-bc9c-ea4463afe751.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

对于`WebMvcConfigurerComposite`类的代码来说，它是一个非常典型的`**<font style="color:#DF2A3F;">组合模式</font>**`。

组合模式的关键点：

1. 组合多个 WebMvcConfigurer 实例：WebMvcConfigurerComposite 通过 delegates 列表组合了多个 WebMvcConfigurer 实例。
2. 统一接口：WebMvcConfigurerComposite 实现了 WebMvcConfigurer 接口，因此可以像一个单一的 WebMvcConfigurer 一样被使用。
3. 代理调用：在实现 WebMvcConfigurer 接口的方法时，WebMvcConfigurerComposite 会遍历 delegates 列表，调用每个 WebMvcConfigurer 实例的相应方法。

总结：WebMvcConfigurerComposite 主要采用了组合模式的思想，将多个 WebMvcConfigurer 实例组合在一起，形成一个整体。

注意：组合模式是GoF 23种设计模式中的结构型设计模式。

# web请求的路径匹配
我们在学习SpringMVC的时候，路径匹配规则中学习了`Ant`风格的路径匹配规则。大家可以翻看一下之前的Spring MVC视频。

在`Spring Boot3`中，对web请求的路径匹配提供了两种规则：

+ 第一种：AntPathMatcher（Ant风格）【**<font style="color:#DF2A3F;">较旧</font>**】
+ 第二种：PathPatternParser（从Spring5.3中引入的。在SpringBoot2.4中引入的。）【**<font style="color:#DF2A3F;">较新：效率高</font>**】
    - **<font style="color:#DF2A3F;">效率比Ant高，一般新项目中使用</font>**`**<font style="color:#DF2A3F;">PathPatternParser</font>**`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

SpringBoot3中默认使用的是`PathPatternParser`，不需要任何配置。如果要使用`AntPathMatcher`，就需要进行如下的配置：

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

## AntPathMatcher
Ant风格的路径匹配规则回顾：

**<font style="color:#DF2A3F;">*</font>**	匹配任意长度的任意字符序列（不包括路径分隔符）。示例：/foo/*.html 匹配 /foo/bar.html 和 /foo/baz.html。

**<font style="color:#DF2A3F;">**</font>**	匹配任意数量的目录层级。示例：/foo/** 匹配 /foo/bar、/foo/bar/baz 和 /foo/bar/baz/qux。

**<font style="color:#DF2A3F;">?</font>****	**匹配**任意单个字符**。示例：/foo?bar 匹配 /foobar 和 /fooxbar。

**<font style="color:#DF2A3F;">[]</font>****	**匹配指定范围内的单个字符。示例：/foo[a-z]bar 匹配 /fooabar、/foobbar 等。

**<font style="color:#DF2A3F;">{}</font>****	**路径变量，用于提取路径的一部分作为参数。示例：/users/{userId} 匹配 /users/123，提取 userId=123。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

如果在SpringBoot3中启用Ant风格，记得配置：

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

如下代码：请分析以下路径匹配的是什么样的路径。

```java
package com.powernode.springboot.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PathController {

    @GetMapping("/{path:[a-z]+}/a?/**/*.do")
    public String path(HttpServletRequest request, @PathVariable String path){
        return request.getRequestURI() + "," + path;
    }
}
```

启动服务器并测试路径：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730472107928-af151fcc-d9b2-439a-825e-660f87f2c952.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## PathPatternParser
项目中不做配置，或者按照以下方式配置，都是`PathPatternParser`：

```properties
spring.mvc.pathmatch.matching-strategy=path_pattern_parser
```

`PathPatternParser`风格是兼容Ant风格的。只有一个地方`PathPatternParser`不支持，Ant支持。在Ant风格中，`**`可以出现在任意位置。在`PathPatternParser`中只允许`**`出现在路径的末尾。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

可以测试一下，将配置文件中的Ant风格注释掉，采用`PathPatternParser`风格。然后控制器代码如下：

```java
package com.powernode.springboot.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PathController {

    @GetMapping("/{path:[a-z]+}/a?/**/*.do")
    public String path(HttpServletRequest request, @PathVariable String path){
        return request.getRequestURI() + "," + path;
    }
}
```

启动服务器报错：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730472557923-8f34cf00-6454-465b-b5e8-562520bbf095.png)

提示你，如果在路径当中出现了`**`，需要将路径匹配规则替换为Ant风格。因此路径当中如果出现`**`，那么必须使用Ant风格。除此之外，`PathPatternParser`均可用。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

我们再来测试一下，`**`放到末尾，对于`PathPatternParser`是否可用？

```java
@GetMapping("/{path:[a-z]+}/a?/*.do/**")
public String path(HttpServletRequest request, @PathVariable String path){
    return request.getRequestURI() + "," + path;
}
```

启动服务器测试，可用：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730472772231-d18e628f-1b62-4757-9299-eaa604344ce6.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 路径匹配相关源码
底层选择路径匹配规则的源码是：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730451084171-157f7728-46f8-4715-8866-dcb1e9215206.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# 内容协商
内容协商机制是Spring MVC框架提供的，接下来主要是学习在SpringBoot中是如何支持SpringMVC内容协商机制的。

## 对内容协商的理解
内容协商机制是指服务器根据客户端的请求来决定返回资源的最佳表示形式。

白话文描述：客户端要什么格式的数据，咱后端就应该返回什么格式的数据。

+ 客户端要JSON，咱就响应JSON。
+ 客户端要XML，咱就响应XML。
+ 客户端要YAML，咱就响应YAML。

你可能会有疑问：客户端接收数据时统一采用一种格式，例如JSON，不就行了吗。哪那么多事儿呀！！！

但在实际的开发中，不是这样的，例如：

+ 遗留的老客户端系统，仍然处理的是XML格式的数据。
+ 要求处理速度快的这种客户端系统，一般要求返回JSON格式的数据。
+ 要求安全性高的客户端系统，一般要求返回XML格式的数据。

因此，在现代的开发中，不同的客户端可能需要后端系统返回不同格式的数据。总之后端应该满足这种多样化的需求。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 实现内容协商的两种方式
通常通过HTTP请求头（如 Accept）或请求参数（如 format）来指定客户端偏好接收的内容类型（如JSON、XML等）。服务器会根据这些信息选择最合适的格式进行响应。

### 通过HTTP请求头（如 Accept）
SpringBoot框架中，在程序员不做任何配置的情况下，优先考虑的是这种方式。

服务器会根据客户端发送请求时提交的请求头中的"Accept: application/json" 或 "Accept: application/xml" 或 "Accept: text/html"来决定响应什么格式的数据。



客户端发送请求给服务器的时候，如何设置请求头的`Accept`？有以下几种常见实现方式：

+ 写代码
    - fetch API
    - ajax的XMLHttpRequest
    - axios库
    - jQuery库......
+ 用工具
    - 接口测试工具，例如：Postman、Apifox等。
    - 命令行工具：curl

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

对于我们编写的以下Controller来说：

```java
package com.powernode.springboot.controller;

import com.powernode.springboot.bean.User;
import com.powernode.springboot.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping(value = "/detail")
    public User detail(){
        return userService.getUser();
    }
}
```

我们使用了`@RestController`，也就是使用了`@ResponseBody`。因此默认支持的是返回JSON数据。怎么才能支持返回XML格式的数据呢？需要做以下两步：

第一步：引入一个依赖

```xml
<dependency>
  <groupId>com.fasterxml.jackson.dataformat</groupId>
  <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

第二步：在实体类上添加一个注解

```java
package com.powernode.springboot.bean;

import com.fasterxml.jackson.dataformat.xml.annotation.JacksonXmlRootElement;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@JacksonXmlRootElement
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private String name;
    private String password;
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

接下来，我们使用`curl`命令行工具，来模拟发送请求，并在请求头中设置`Accept`：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730516542448-4168db70-6fcb-456a-ae96-8004f5ba69af.png)

可以很清楚的看到，服务器根据不同的请求头返回了不同格式的数据。

+ `Accept: application/xml`则返回XML格式的数据
+ `Accept: application/json`则返回JSON格式的数据

### <font style="color:rgb(44, 44, 54);">通过请求参数（如 </font>`<font style="color:rgb(44, 44, 54);">format</font>`<font style="color:rgb(44, 44, 54);">）</font>
接下来我们使用请求参数的方式，来测试一下服务器的响应，注意：默认的请求参数名为`format`。

我们仍然使用`curl`命令行工具进行测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730516739451-f4786761-88a5-4b00-ba68-c310bceafec0.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

我们可以看到，并没有达到我们想要的效果，这是因为`SpringBoot优先考虑的不是通过请求参数format方式`。如何优先考虑使用`format`方式呢？做如下配置：

```properties
# 内容协商时，优先考虑请求参数format方式。
spring.mvc.contentnegotiation.favor-parameter=true
```

再次测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730516907141-61b292a9-1e9f-4248-9da6-34b26ddf8bd0.png)

可以看到，现在SpringBoot已经优先考虑使用`请求参数format`方式了。



当然，请求参数的名字可以不使用`format`吗？支持定制化吗？答案是支持的，例如你希望请求参数的名字为`type`，可以做如下配置：

```properties
# 内容协商时，设置请求参数的名字，默认为format
spring.mvc.contentnegotiation.parameter-name=type
```

再次使用`curl`工具进行测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730517197351-b400c853-b45b-474b-9380-53d4d901d697.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## HttpMessageConverter接口
### HttpMessageConverter的理解
`HttpMessageConverter`接口，对于这个接口来说，大家应该不陌生，在Spring MVC的课程当中咱们已经详细的学习过了。在这里简单回顾一下。

`HttpMessageConverter`接口被翻译为：`Http消息转换器`。它起到转换`Http消息`的作用。

什么是`Http消息`？所谓的`Http消息`本质上就是浏览器向服务器发送请求时提交的数据，或者服务器向浏览器响应的数据。

而`HttpMessageConverter`接口就是负责完成`请求/响应`时的数据格式转换的。

在Spring MVC中提供了很多`HttpMessageConverter`接口的实现类，不同的`Http消息转换器`具有不同的转换效果，有的负责将Java对象转换为JSON格式的字符串，有的负责将Java对象转换成XML格式的字符串。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 常见的HttpMessageConverter
内置的常见的`HttpMessageConverter`的实现类包括：

+ <font style="color:#DF2A3F;">【请求】提交的表单（form）数据转换成Java对象的主要任务是由 FormHttpMessageConverter 消息转换器完成的</font>
+ <font style="color:#DF2A3F;">【请求】提交的JSON数据转换成Java对象的主要任务是由 MappingJackson2HttpMessageConverter 消息转换器完成的。（我们通常使用的</font>`<font style="color:#DF2A3F;">@RequestBody</font>`<font style="color:#DF2A3F;">注解）</font>
+ 【响应】将Java对象转换成JSON格式的数据，并将其写入HTTP响应体的任务是由 MappingJackson2HttpMessageConverter 消息转换器完成。（我们通常使用的`@ResponseBody`注解)
+ 【响应】将Java对象转换成XML格式的数据，并将其写入HTTP响应体的任务通常由 Jaxb2RootElementHttpMessageConverter 消息转换器完成。
+ 【响应】将 String 直接写入到响应体的任务是由 StringHttpMessageConverter 消息转换器完成。
+ ......



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 请求时通过哪些条件确定使用哪个转换器
请求时通常根据以下条件来确定使用哪个消息转换器：

1. 请求的 Content-Type 字段：

Spring MVC 会检查请求的 Content-Type 字段，以确定请求体的数据格式（例如 application/json、application/x-www-form-urlencoded、application/xml 等）。

2. 方法参数类型：

控制器方法中接收请求体的参数类型（例如 @RequestBody）。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 响应时通过哪些条件确定使用哪个转换器
响应时通常根据以下条件来确定使用哪个消息转换器：

1. 请求提交时，请求头上的Accept字段 ：

Spring MVC 会检查客户端请求的 Accept 字段，以确定客户端期望的响应格式（例如 application/json、application/xml 等）。

2. 方法返回值类型：

控制器方法的返回值类型（例如 @ResponseBody）。

例如1：@ResponseBody + 控制器方法的返回值是String，则使用StringHttpMessageConverter转换器。（将字符串直接写入响应体）

例如2：@ResponseBody + 控制器方法的返回值是User，则使用MappingJackson2HttpMessageConverter转换器。（将java对象转换成json格式的字符串写入到响应体）



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 系统默认提供了哪些HttpMessageConverter
查看源码：

WebMvcAutoConfiguration.EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport

`WebMvcAutoConfiguration`内部类`EnableWebMvcConfiguration`

`EnableWebMvcConfiguration`继承了`DelegatingWebMvcConfiguration`

`DelegatingWebMvcConfiguration`继承了`WebMvcConfigurationSupport`



在`WebMvcConfigurationSupport`类中有这样一个方法：`addDefaultHttpMessageConverters()` 用来添加默认的`HttpMessageConverter`对象。

通过断点调试，可以发现默认支持6个HttpMessageConverter，如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730774897755-ad87ddc8-4700-40e4-baee-a28d94000e7a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

这6个`HttpMessageConverter`作用如下：

1. **ByteArrayHttpMessageConverter:**

用于将字节数组(byte[])与HTTP消息体之间进行转换。这通常用于处理二进制数据，如图片或文件。

2. **StringHttpMessageConverter:**

用于将字符串(String)与HTTP消息体之间进行转换。它支持多种字符集编码，能够处理纯文本内容。

3. **ResourceHttpMessageConverter:**

用于将Spring的Resource对象与HTTP消息体之间进行转换。Resource是Spring中表示资源的接口，可以读取文件等资源。这个转换器对于下载文件或发送静态资源有用。

4. **ResourceRegionHttpMessageConverter:**

用于处理资源的部分内容（即“Range”请求），特别是当客户端请求大文件的一部分时。这对于实现视频流媒体等功能很有用。

5. **AllEncompassingFormHttpMessageConverter:**

用于处理表单，是一个比较全面的form消息转换器。处理标准的application/x-www-form-urlencoded格式的数据，以及包含文件上传的multipart/form-data格式的数据。

6. **MappingJackson2HttpMessageConverter:**

使用Jackson库来序列化和反序列化JSON数据。可以将Java对象转换为JSON格式的字符串，反之亦然。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

另外，通过以下源码，也可以看到SpringBoot是根据类路径中是否存在某个类，而决定是否添加对应的消息转换器的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730775823305-dfc2b0c5-6ca1-4e8f-902b-506e3d86246d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730775878229-960b3144-1727-4467-899a-6bd16f6bc965.png)

因此，我们只要引入相关的依赖，让类路径存在某个类，则对应的消息转换器就会被加载。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 定义自己的HttpMessageConverter
可以看到以上6个消息转换器中没有yaml相关的消息转换器，可见，如果要实现yaml格式的内容协商，yaml格式的消息转换器就需要我们自定义了。

### 第一步：引入能够处理yaml格式的依赖
任何一个能够处理yaml格式数据的库都可以，这里选择使用`jackson`的库，因为它既可以处理json，xml，又可以处理yaml。

```xml
<dependency>
  <groupId>com.fasterxml.jackson.dataformat</groupId>
  <artifactId>jackson-dataformat-yaml</artifactId>
</dependency>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

编写测试程序，简单测试一下这个库的用法：

```java
package com.powernode.springboot;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.dataformat.yaml.YAMLFactory;
import com.fasterxml.jackson.dataformat.yaml.YAMLGenerator;
import com.powernode.springboot.bean.User;

public class Jackson2YamlTest {
    public static void main(String[] args) throws JsonProcessingException {
        // 创建YAML工厂类
        YAMLFactory yamlFactory = new YAMLFactory().disable(YAMLGenerator.Feature.WRITE_DOC_START_MARKER); // 禁止使用文档头标记
        // 创建对象映射器
        ObjectMapper objectMapper = new ObjectMapper(yamlFactory);
        // 准备数据
        User user = new User("jackson", "jack123");
        // 将数据转换成YAML格式
        String s = objectMapper.writeValueAsString(user);
        System.out.println(s);
    }
}
```

执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730778662876-bf7851a4-1e8e-4d64-9584-443fd71cef7e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 第二步：新增一种媒体类型yaml
默认支持xml和json两种媒体类型，要支持yaml格式的，需要新增一个yaml媒体类型，在springboot的配置文件中进行如下配置：

```properties
spring.mvc.contentnegotiation.media-types.yaml=text/yaml
```

注意，以上`types`后面的`yaml`是媒体类型的名字，名字随意，如果媒体类型起名为`xyz`，那么发送请求时的路径应该是这样的：http://localhost:8080/detail?format=xyz

### 第三步：自定义HttpMessageConverter
编写类`YamlHttpMessageConverter`继承`AbstractHttpMessageConverter`，代码如下：

```java
package com.powernode.springboot.config;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.dataformat.yaml.YAMLFactory;
import com.fasterxml.jackson.dataformat.yaml.YAMLGenerator;
import com.powernode.springboot.bean.User;
import org.springframework.http.HttpInputMessage;
import org.springframework.http.HttpOutputMessage;
import org.springframework.http.MediaType;
import org.springframework.http.converter.AbstractHttpMessageConverter;
import org.springframework.http.converter.HttpMessageNotReadableException;
import org.springframework.http.converter.HttpMessageNotWritableException;

import java.io.IOException;
import java.nio.charset.Charset;

public class YamlHttpMessageConverter extends AbstractHttpMessageConverter<Object> {

    private ObjectMapper objectMapper = new ObjectMapper(new YAMLFactory().disable(YAMLGenerator.Feature.WRITE_DOC_START_MARKER));

    public YamlHttpMessageConverter() {
        // 让 消息转换器 和 媒体类型 application/yaml 绑定在一起。
        super(new MediaType("text", "yaml", Charset.forName("UTF-8")));
    }

    @Override
    protected boolean supports(Class<?> clazz) {
        // 表示User类型的数据支持yaml，其他类型不支持
        return User.class.isAssignableFrom(clazz);
    }

    // 处理 @RequestBody（将提交的yaml格式数据转换为java对象）
    @Override
    protected Object readInternal(Class<?> clazz, HttpInputMessage inputMessage) throws IOException, HttpMessageNotReadableException {
        return null;
    }

    // 处理 @ResponseBody（将java对象转换为yaml格式的数据）
    @Override
    protected void writeInternal(Object o, HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException {
        this.objectMapper.writeValue(outputMessage.getBody(), o);
        // 注意：spring框架会自动关闭输出流，无需程序员手动释放。
    }
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 第四步：配置消息转换器
重写`WebMvcConfigurer`接口的`configureMessageConverters`方法：

```java
package com.powernode.springboot.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.List;

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(new YamlHttpMessageConverter());
    }
}
```

启动服务器并测试：http://localhost:8080/detail?type=yaml

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730783005228-fe11e134-539e-4291-a12a-f2f83f9b9705.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# SpringBoot整合Thymeleaf
## 传统web应用和前后端分离
如果你是做前后端分离的项目，这一章节的内容将用不上。

现代开发大部分应用都会采用前后端分离的方式进行开发，前端是一个独立的系统，后端也是一个独立的系统，后端系统只给前端系统提供数据（JSON数据），不需要后端解析模板页面，前端系统拿到后端提供的数据之后，前端负责填充数据即可。因此这一章节内容作为了解。



传统的WEB应用（非前后端分离）：浏览器页面上展示成什么效果，后端服务器说了算，这是传统web应用最大的特点。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730789733440-1dc2d749-19c5-4363-b35d-bf6e6008265c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

前后端分离的应用：前端是一个独立的系统，后端也是一个独立的系统，后端系统不再负责页面的渲染，后端系统只负责给前端系统提供开放的API接口，后端系统只负责数据的收集，然后将数据以JSON/XML等格式响应给前端系统。前端系统拿到接口返回的数据后，将数据填充到页面上。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730790760614-212e3911-eb37-4dca-88a4-eca6ecd26dba.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

前后端分离的好处：

+ 职责清晰：前端专注于用户界面和用户体验，后端专注于业务逻辑和数据处理。
+ 开发效率高：前后端可以并行开发，互不影响，提高开发速度。
+ 可维护性强：代码结构更清晰，便于维护和扩展。
+ 技术栈灵活：前后端可以独立选择最适合的技术栈。
+ 响应式设计：前端可以更好地处理不同设备和屏幕尺寸。
+ 性能优化：前后端可以独立优化，提升整体性能。
+ 易于测试：前后端接口明确，便于单元测试和集成测试。



## SpringBoot整合Thymeleaf
Java的模板技术有很多，SpringBoot支持以下的模板技术：

1. **Thymeleaf**：
    - **特点**：Thymeleaf 是一个现代的服务器端Java模板引擎，它支持HTML5，XML，TEXT，JAVASCRIPT，CSS等多种模板类型。它能够在浏览器中预览，这使得前端开发更加便捷。Thymeleaf 提供了一套强大的表达式语言，可以轻松地处理数据绑定、条件判断、循环等。
    - **优势**：**<font style="color:#DF2A3F;">与Spring框架集成良好，也是SpringBoot官方推荐的</font>**。
2. **FreeMarker**：
    - **特点**：FreeMarker 是一个用Java编写的模板引擎，主要用来生成文本输出，如HTML网页、邮件、配置文件等。它不依赖于Servlet容器，可以在任何环境中运行。
    - **优势**：模板语法丰富，灵活性高，支持宏和函数定义，非常适合需要大量定制化的项目。
3. **Velocity**：
    - **特点**：Velocity 是另一个强大的模板引擎，最初设计用于与Java一起工作，但也可以与其他技术结合使用。它提供了简洁的模板语言，易于学习和使用。
    - **优势**：轻量级，性能优秀，特别适合需要快速生成静态内容的应用。
4. **Mustache**：
    - **特点**：Mustache 是一种逻辑无感知的模板语言，可以用于多种编程语言，包括Java。它的设计理念是让模板保持简单，避免模板中出现复杂的逻辑。
    - **优势**：逻辑无感知，确保模板的简洁性和可维护性，易于与前后端开发人员协作。
5. **Groovy Templates**：
    - **特点**：Groovy 是一种基于JVM的动态语言，它可以作为模板引擎使用。Groovy Templates 提供了非常灵活的模板编写方式，可以直接嵌入Groovy代码。
    - **优势**：对于熟悉Groovy语言的开发者来说，使用起来非常方便，可以快速实现复杂逻辑。

这些模板技术各有千秋，选择哪一种取决于项目的具体需求和个人偏好。Spring Boot 对这些模板引擎都提供了良好的支持，通常只需要在项目中添加相应的依赖，然后按照官方文档配置即可开始使用。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



<font style="color:#DF2A3F;">提醒：SpringBoot内嵌了Servlet容器（例如：Tomcat、Jetty等），使用SpringBoot不太适合使用JSP模板技术，因为SpringBoot项目最终打成jar包之后，放在jar包中的jsp文件不能被Servlet容器解析。</font>



要在SpringBoot中整合Thymeleaf，按照以下步骤操作：

第一步：引入thymeleaf启动器

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

第二步：编写配置文件，指定前缀和后缀（**<font style="color:#DF2A3F;">默认不配置就是以下配置</font>**）

```properties
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

第三步：编写控制器

```java
package com.powernode.springboot.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

// 不能使用 @RestController
@Controller
public class HelloController {

    @GetMapping("/h")
    public String helloThymeleaf(@RequestParam("name") String name, Model model) {
        // 将接收到的name数据存储到域对象中
        model.addAttribute("name", name);
        // 逻辑视图名
        return "hello"; // 最终的物理视图名：classpath:/templates/hello.html
    }
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

第四步：编写thymeleaf模板页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>hello, thymeleaf</title>
</head>
<body>
<h1>hello,<span th:text="${name}"></span></h1>
</body>
</html>
```

启动服务器，测试地址为：http://localhost:8080/h

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792132534-7b9358f0-98ff-43dc-8624-1717683b15aa.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## Thymeleaf的自动配置
Thymeleaf的自动配置类：`ThymeleafAutoConfiguration`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792250202-195d729c-66e5-46e9-b86e-6cf9494ef895.png)

我们通过查看上图中红框中的类，可以找到thymeleaf的相关配置：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792335100-efa4c9cf-96c4-456d-92f6-71ceaf7f9452.png)

与thymeleaf相关的配置统一使用`spring.thymeleaf`即可。并且通过源码可以进一步了解到默认的前缀和后缀：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730792465391-d516ec42-a8da-46f4-8325-197617074a6a.png)

也就是说，默认情况下，只要放在`classpath:/templates/`目录下的`xxx.html`会被自动当做为thymeleaf的模板文件被thymeleaf模板引擎解析。因此放在`classpath:/templates/`目录下的`html`文件不是静态页面，而是动态的thymeleaf模板页面。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## Thymeleaf核心语法
### th:text 替换标签体内容
注意：在根标签<html>中引入 xmlns:th="[http://www.thymeleaf.org"](http://www.thymeleaf.org")，在编写`th:`语法时有智能提示。

`th:text`用来替换标签体内容的，例如：

```html
th:text 语法：替换标签体中的内容
<div th:text="${name}">我是一个DIV</div>
```

运行效果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730794669882-1e70cf45-36bf-456f-97a1-6b1fb3b45f30.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)



提示：标签体中的内容即使是一段HTML代码，也只是会被当做普通文本对待。例如我们让存储在域中的文本内容是一段HTML代码：

```java
model.addAttribute("htmlCode", "<a href='http://www.bjpowernode.com'>动力节点</a>");
```

```html
<div th:text="${htmlCode}">我是一个DIV</div>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730795096806-e6cf0de5-6295-4e67-990b-97a9ace01976.png)

### th:utext 替换标签体内容
作用和 `th:text`一样。只不过`th:utext`会将内容当做一段HTML代码解析并替换。将以上测试代码修改为：

```html
th:utext 语法：替换标签体中的内容
<div th:utext="${htmlCode}">我是一个DIV</div>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730795321767-2f90cc02-87da-4269-aae1-26bb3fa0f939.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### th:任意属性名 动态替换该属性的值
例如：我们向域中存储以下数据

```java
// 向域中存储一个html标签的某个属性的值
model.addAttribute("company", "动力节点");
model.addAttribute("hrefValue", "http://www.bjpowernode.com");
```

然后使用`th:href`动态替换`href`属性的值：

```html
th:任意属性名 语法：动态替换属性值
<a th:href="${hrefValue}" href="https://www.baidu.com" th:text="${company}">百度</a>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730795806734-d453db1e-86d3-4ecf-affb-cdff95491241.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### th:attr 属性合并设置
+ 分开设置：

```java
model.addAttribute("hrefValue", "http://www.bjpowernode.com");
model.addAttribute("style", "color:red");
```

```html
<a th:href="${hrefValue}" th:style="${style}">动力节点</a>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730796198283-c5a66c03-144d-4338-8d86-4b252747bc1f.png)

+ 合并设置：使用`th:attr`

```java
model.addAttribute("hrefValue", "http://www.bjpowernode.com");
model.addAttribute("style", "color:red");
```

```html
<a th:attr="href=${hrefValue},style=${style}">动力节点</a>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### th:指令
指令非常多，具有代表性的例如：`th:if`，该指令用来控制元素`隐藏`和`显示`。

在`static`静态资源目录下存放一张图片：dog1.jpg

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730796605419-b6da6632-95e2-4b77-bb64-e64cc1a50e06.png)

然后编写这样的模板代码：

```html
<img src="dog1.jpg" th:if="true">
```

测试结果，图片是显示的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730796706035-255d756e-2806-4f41-8ef8-83c1412ae091.png)

如果`th:if`的值修改为`false`，我们会发现隐藏了。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### @{} 表达式
`${}`表达式语法是专门用来获取`model`中绑定的数据的。

`@{}`表达式语法是专门用来维护URL请求路径的。它可以动态设置项目的根路径。



SpringBoot中默认的项目根路径是：`/`

假设我们编写这样的java代码，向model中绑定一个路径：

```java
model.addAttribute("imgUrl", "/dog1.jpg");
```

我们编写这样的模板代码：

```html
<img th:src="${imgUrl}">
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730797446050-1a8e29e1-6339-4311-8420-73dee00a3d3d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

此时是可以正常显示的，但如果我们将web应用的根路径进行了修改，将其配置为：`/myweb`

```properties
server.servlet.context-path=/myweb
```

其他位置代码不做修改，我们再来访问页面，注意访问路径中要添加`/myweb`，例如：[http://localhost:8080/myweb/h?name=jackson](http://localhost:8080/myweb/h?name=jackson)

访问后使用`ctrl + F5`强行刷新，不走浏览器缓存，结果发现无法访问到该图片：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730797689412-190d1f6c-7cb2-42c0-9414-30aa6550e699.png)



此时我们将模板代码进行如下修改，将`${}`修改为`@{}`：

```html
<img th:src="@{${imgUrl}}">
```

再次测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730797801499-a27a88b1-9024-4e0f-90fe-d2a64440d41c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### thymeleaf的内置工具
内置工具很多，可以参考官方文档：[https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#strings](https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#strings)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798338220-625af768-8576-4d24-a89d-46eaacef936c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

例如我们要完成这样一个效果，用户提交用户名时，如果名字中含有`jack`则显示`狗狗图片`，否则不显示。

模板代码如下：

```html
<img th:src="@{${imgUrl}}" th:if="${#strings.contains(name,'jack')}">
```

测试结果1：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798493853-7e6e9545-f7dd-43b3-b157-7c853fb2da66.png)

测试结果2：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798509202-cb67e7fe-f8ab-487e-ba9e-470ab50dcfbf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### thymeleaf也支持运算符
例如`>`运算符，我们实现这样一个功能，如果提供的用户名长度大于6，则显示狗狗图片：

```html
<img th:src="@{${imgUrl}}" th:if="${#strings.length(name) > 6}">
```

测试结果1：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798722013-6be35719-74db-4349-86d9-d5c8b99f0d5b.png)

测试结果2：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798741960-7e1c643d-9ac1-4796-972e-b61f23b988d4.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

另外thymeleaf也支持三目运算符：

```html
<span th:text="${user.gender ? '男' : '女'}"></span>
```

如果性别是true，则显示男，false，则显示女。



还有很多其他的运算符，可参考thymeleaf官方文档。



### thymeleaf的字符串拼接
第一种方式：使用加号 `+`

```html
<div th:text="${'姓名：' + name + '，年龄：18'}"></div>
```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730798992476-8176fe7d-332f-4806-be1a-84923463232e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

第二种方式：使用竖线 `||`

```html
<div th:text="|姓名：${name}，年龄：18|"></div>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730799059381-eb2cc823-306a-4f52-9ae5-0012783c8f14.png)

### 循环遍历
代码示例：

```html
<tr th:each="user : ${users}">
  <td th:text="${user.name}"></td>
  <td th:text="${user.age}"></td>
  <td th:text="${user.gender}"></td>
  <td th:text="${user.desc}"></td>
  <td th:text="${user.location}"></td>
</tr>
```

说明：

+ ${users}：代表集合
+ user：代表集合中的每个元素
+ ${user.name}：元素的name属性值

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

遍历时也可以添加状态对象，代码示例：

```html
<tr th:each="user,state : ${userList}">
    <td th:text="${state.count}">1</td>
    <td th:text="${user.name}">张三</td>
    <td th:text="${user.age}">20</td>
    <td th:text="${user.gender}"></td>
    <td th:text="${user.desc}"></td>
    <td th:text="${user.location}"></td>
    <td>
        thymeleaf的内联表达式：<br>
        下标：[[${state.index}]]<br>
        序号：[[${state.count}]]<br>
        当前对象：[[${state.current}]]<br>
        元素总数：[[${state.size}]]<br>
        是否为偶数行：[[${state.even}]]<br>
        是否为奇数行：[[${state.odd}]]<br>
        是否第一个元素：[[${state.first}]]<br>
        是否最后一个元素：[[${state.last}]]<br>
    </td>
</tr>
```



注意：以上`[[${state.index}]]`这种语法属于thymeleaf中的`内联表达式`写法。也可以写成：`[(${state.index})]`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

另外，状态对象`state`的属性包括：

+ index：下标
+ count：序号
+ current：当前对象
+ size：元素总数
+ even：是否为偶数行
+ odd：是否为奇数行
+ first：是否为第一个元素
+ last：是否为最后一个元素

### 条件判断th:if
th:if 语法用来决定元素是否显示：true显示。false隐藏。

<div th:if="true">我是一个div元素</div>，则显示该div

<div th:if="false">我也是一个div元素</div>，则隐藏该div



实现这样一个功能：用户如果没有留下简介，则显示`你比较懒没有留下任何介绍信息`，如果留下了简介，则显示具体的简介信息。

```html
<td th:if="${#strings.isEmpty(user.desc)}" th:text="'你比较懒没有留下任何介绍信息'"></td>
<td th:if="${not #strings.isEmpty(user.desc)}" th:text="${user.desc}"></td>
```

### 条件判断th:switch
实现一个这样的功能：如果城市编号001则显示北京，002则显示上海，003则显示广州，004则显示深圳，其他值显示未知。

```html
<td th:switch="${user.location}">
  <span th:case="001">北京</span>
  <span th:case="002">上海</span>
  <span th:case="003">广州</span>
  <span th:case="004">深圳</span>
  <span th:case="*">未知</span>
</td>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### thymeleaf属性优先级
thymeleaf的属性优先级非常重要，因为它直接决定了模板的解析和执行顺序。

以下是Thymeleaf属性的优先级从高到低的列表，以表格形式展示：

| 优先级 | 属性 | 描述 |
| --- | --- | --- |
| 1 | `th:if` | 如果条件为真，则渲染该元素。 |
| 2 | `th:unless` | 如果条件为假，则渲染该元素。 |
| 3 | `th:with` | 定义局部变量。 |
| 4 | `th:switch` | 开始一个 switch 语句。 |
| 5 | `th:case` | 定义 switch 语句中的 case 分支。 |
| 6 | `th:each` | 遍历列表，用于循环。 |
| 7 | `th:remove` | 移除元素或其属性。 |
| 8 | `th:attr` | 设置或修改元素的属性。 |
| 9 | `th:classappend` | 追加 CSS 类。 |
| 10 | `th:styleappend` | 追加内联样式。 |
| 11 | `th:src` | 设置元素的 `src` 属性。 |
| 12 | `th:href` | 设置元素的 `href` 属性。 |
| 13 | `th:value` | 设置元素的 `value` 属性。 |
| 14 | `th:text` | 设置元素的文本内容。 |
| 15 | `th:utext` | 设置元素的未转义文本内容。 |
| 16 | `th:html` | 设置元素的 HTML 内容。 |
| 17 | `th:fragment` | 定义模板片段。 |
| 18 | `th:insert` | 插入一个模板片段。 |
| 19 | `th:replace` | 替换当前元素为一个模板片段。 |
| 20 | `th:include` | 包含一个模板片段的内容。 |
| 21 | `th:block` | 用于逻辑分组，不产生任何HTML输出。 |


![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

对于thymeleaf属性优先级，我总结了以下一段话，把它记住即可：

**“先控制，再遍历，后操作，末内容。”**

具体来说：

1. **先控制**：`th:if` 和 `th:unless` 用于条件控制，决定是否渲染元素。
2. **再遍历**：`th:each` 用于遍历列表，生成多个元素。
3. **后操作**：`th:with`、`th:switch`、`th:case`、`th:remove`、`th:attr` 等用于局部变量定义、条件分支、属性操作等。
4. **末内容**：`th:text`、`th:utext`、`th:html` 等用于设置元素的内容。

### *{...} 表达式
`*{...}` 主要用于在上下文中访问对象的属性。这种表达式通常在表单处理和对象绑定场景中使用。



**语法**：`*{property}`：访问当前**<font style="color:#DF2A3F;">上下文</font>**对象的某个属性。



**使用场景**

+ 表单绑定：在表单中绑定对象的属性。
+ 对象属性访问：在模板中访问对象的属性，特别是当对象是当前上下文的一部分时。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**示例**

1. **表单绑定**

假设你有一个 `User` 对象，包含 `name` 和 `age` 属性，你可以在表单中使用 `*{...}` 表达式来绑定这些属性：

```html
<form th:object="${user}" method="post" action="/submit">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" th:field="*{name}" /> 
    <label for="age">Age:</label>
    <input type="number" id="age" name="age" th:field="*{age}" />
    <button type="submit">Submit</button>
</form>
```

在这个例子中：

+ `th:object="${user}"` 将 `user` 对象设置为当前上下文对象。
+ `th:field="*{name}"` 和 `th:field="*{age}"` 分别绑定到 `user` 对象的 `name` 和 `age` 属性。



2. **对象属性访问**

假设你有一个 `User` 对象，包含 `name` 和 `age` 属性，你可以在模板中使用 `*{...}` 表达式来访问这些属性：

```html
<div th:object="${user}">
    <p>Name: <span th:text="*{name}">Default Name</span></p>
    <p>Age: <span th:text="*{age}">Default Age</span></p>
</div>

```

在这个例子中：

+ `th:object="${user}"` 将 `user` 对象设置为当前上下文对象。
+ `*{name}` 和 `*{age}` 分别访问 `user` 对象的 `name` 和 `age` 属性。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**与 **`**${...}**`** 的区别**

+ `${...}`：标准表达式，用于访问模型中的变量和执行简单的表达式。
+ `*{...}`：属性选择表达式，用于在上下文中访问对象的属性，通常与 `th:object` 一起使用。

### 代码片段共享
片段是Thymeleaf中用于代码复用的基本机制。你可以将共享的部分提取到单独的HTML文件中，然后在其他模板中引用这些片段。



页面中公共的header.html

```html
<header th:fragment="h">
    <nav>
        <ul>
            <li><a th:href="@{/a}">Home</a></li>
            <li><a th:href="@{/b}">About</a></li>
        </ul>
    </nav>
</header>
```

页面中公共的footer.html

```html
<footer th:fragment="f">
  <p>&copy; 2024 北京动力节点</p>
</footer>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

在`a.html`中包含以上两个公共部分：

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>主页</title>
</head>
<body>
<div th:replace="~{header :: h}"></div>

<main>
    <h1>欢迎来到主页</h1>
    <p>这是主页的主要内容.</p>
</main>

<div th:replace="~{footer :: f}"></div>
</body>
</html>
```

在`b.html`中包含以上两个公共部分：

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>关于我们</title>
</head>
<body>

<div th:replace="~{header :: h}"></div>

<main>
    <h1>关于我们</h1>
    <p>动力节点专注IT培训16年.</p>
</main>

<div th:replace="~{footer :: f}"></div>
</body>
</html>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

主要作用是代码复用。实现此功能的主要代码：

+ 在公共代码部分使用`th:fragment="片段名称"`来声明公共代码片段的名字。
+ 在需要引入的地方使用`th:replace="~{文件名去掉后缀 :: 片段名称}"`来引入。



**<font style="color:#DF2A3F;">小插曲：在springboot中如何实现：直接将请求路径映射到特定的视图，而不需要编写controller？</font>**

+ 第一步：视图解析器配置

```properties
spring.mvc.view.prefix=/templates/
spring.mvc.view.suffix=.html
```

+ 第二步：使用`ViewControllerRegistry`进行视图与控制器的注册

```java
package com.powernode.springboot.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/a").setViewName("a");
        registry.addViewController("/b").setViewName("b");
    }
}

```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## thymeleaf页面修改如何立即生效
第一步：引入springboot提供的`dev tools`

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
  <scope>runtime</scope>
  <optional>true</optional>
</dependency>
```

第二步：关闭应用重启功能，如果不关闭会导致每一次修改java代码后立即重启应用，不建议。我们现在只希望做到的功能是，修改thymeleaf模板文件后立即生效。

```properties
spring.devtools.restart.enabled=false
```

第三步：修改代码后在IDEA中按组合键`ctrl+f9`



以上三步配合即可。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# 异常处理
在controller层如果程序出现了异常，并且这个异常未被捕获，springboot提供的异常处理机制将生效。

Spring Boot 提供异常处理机制主要是为了提高应用的健壮性和用户体验。它的好处包括：

1. **统一错误响应**：可以定义全局异常处理器来统一处理各种异常，确保返回给客户端的错误信息格式一致，便于前端解析。
2. **提升用户体验**：能够优雅地处理异常情况，避免直接将技术性错误信息暴露给用户，而是显示更加友好的提示信息。
3. **简化代码**：开发者不需要在每个可能抛出异常的方法中重复编写异常处理逻辑，减少冗余代码，使业务代码更加清晰简洁。
4. **增强安全性**：通过控制异常信息的输出，防止敏感信息泄露，增加系统的安全性。

## 自适应的错误处理机制
springboot会根据请求头的Accept字段来决定错误的响应格式。

这种机制的好处就是：客户端设备自适应，提高用户的体验。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731424148788-8a9c52cc-7683-479a-a8b8-9075d779bc4e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731424190201-4c9b9781-08ae-48f7-8b4a-9956b3b700d7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## SpringMVC的错误处理方案
**<font style="color:#DF2A3F;">重点：如果程序员使用了SpringMVC的错误处理方案，SpringBoot的错误处理方案不生效。</font>**

### 局部控制 @ExceptionHandler
在控制器当中编写一个方法，方法使用@ExceptionHandler注解进行标注，凡是**这个控制器**当中出现了**对应的异常**，则走这个方法来进行异常的处理。局部生效。

```java
package com.powernode.test.controller;

import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @GetMapping("/resource/{id}")
    public String getResource(@PathVariable Long id){
        if(id == 1){
            throw new IllegalArgumentException("无效ID：" + id);
        }
        return "ID = " + id;
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public String handler(IllegalArgumentException e){
        return "错误信息：" + e.getMessage();
    }
}

```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

可以再编写一个OtherController，让它也发生`IllegalArgumentException`异常，看看它会不会走局部的错误处理机制。

```java
package com.powernode.test.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class OtherController {
    @GetMapping("/resource2/{id}")
    public String getResource(@PathVariable Long id){
        if(id == 1){
            throw new IllegalArgumentException("无效ID：" + id);
        }
        return "ID = " + id;
    }
}

```

通过测试，确实局部生效。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 全局控制 @ControllerAdvice + @ExceptionHandler
也可以把以上局部生效的方法单独放到一个类当中，这个类使用@ControllerAdvice注解标注，凡是**任何控制器**当中出现了**对应的异常**，则走这个方法来进行异常的处理。全局生效。

将之前的局部处理方案的代码注释掉。使用全局处理方式，编写以下类：

```java
package com.powernode.test.controller;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    @ResponseBody
    public String handler(IllegalArgumentException e){
        return "错误信息：" + e.getMessage();
    }
}

```

通过测试，确实全局生效。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## SpringBoot的错误处理方案
**<font style="color:#DF2A3F;">重点：如果SpringMVC没有对应的处理方案，会开启SpringBoot默认的错误处理方案。</font>**

SpringBoot默认的错误处理方案如下：

1. 如果客户端要的是json，则直接响应json格式的错误信息。
2. 如果客户端要的是html页面，则按照下面方案：
+ 第一步（精确错误码文件）：去`classpath:/templates/error/`目录下找`404.html``500.html`等`精确错误码.html`文件。如果找不到，则去静态资源目录下的/error目录下找。如果还是找不到，才会进入下一步。
+ 第二步（模糊错误码文件）：去`classpath:/templates/error/`目录下找`4xx.html``5xx.html`等`模糊错误码.html`文件。如果找不到，则去静态资源目录下的/error目录下找。如果还是找不到，才会进入下一步。
+ 第三步（通用错误页面）：去找`classpath:/templates/error.html`如果找不到则进入下一步。
+ 第四步（默认错误处理）：如果上述所有步骤都未能找到合适的错误页面，Spring Boot 会使用内置的默认错误处理机制，即 `/error` 端点。

## 如何在错误页获取错误信息
Spring Boot 默认会在模型Model中放置以下信息：

+ timestamp: 错误发生的时间戳
+ status: HTTP 状态码
+ error: 错误类型（如 "Not Found"）
+ exception: 异常类名
+ message: 错误消息
+ trace: 堆栈跟踪

在thymeleaf中使用 `${message}`即可取出信息。



注意：**<font style="color:#DF2A3F;">springboot3.3.5</font>**版本默认只向Model对象中绑定了`timestamp``status``error`。如果要保存`exception``message``trace`，需要开启以下三个配置：

```plain
server.error.include-stacktrace=always
server.error.include-exception=true
server.error.include-message=always
```



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 前后端分离项目的错误处理方案
统一使用SpringMVC的错误处理方案，定义全局的异常处理机制：@ControllerAdvice + @ExceptionHandler

返回json格式的错误信息，其它的就不需要管了，因为前端接收到错误信息怎么处理是他自己的事儿。

## 服务器端负责页面渲染的项目错误处理方案
建议使用SpringBoot的错误处理方案：

1. 如果发生的异常是HTTP错误状态码：
    1. 建议常见的错误码给定`精确错误码.html`
    2. 建议不常见的错误码给定`模糊错误码.html`
2. 如果发生的异常不是HTTP错误状态码，而是业务相关异常：
    1. 在程序中处理具体的业务异常，自己通过程序来决定跳转到哪个错误页面。
3. 建议提供`classpath:/templates/error.html`来处理通用错误。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# 国际化（了解）
在Spring Boot中实现国际化（i18n）是一个常见的需求，它允许应用程序根据用户的语言和地区偏好显示不同的文本。

## 实现国际化
### 第一步：创建资源文件
创建包含不同语言版本的消息文件。这些文件通常放在`src/main/resources`目录下，并且以`.properties`为扩展名。例如：

+ `messages.properties` (默认语言，如英语)
+ `messages_zh_CN.properties` (简体中文)
+ `messages_fr.properties` (法语)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

每个文件都应包含相同的消息键，但值应对应于相应的语言。例如：

**messages.properties**:

```properties
welcome.message=Welcome to our application!
```

**messages_zh_CN.properties**:

```properties
welcome.message=欢迎来到我们的应用！
```

**messages_fr.properties**:

```properties
welcome.message=Bienvenue dans notre application !
```



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 第二步：在模板文件中取出消息
语法格式为：#{welcome.message}

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="#{welcome.message}"></h1>
</body>
</html>
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

**测试1：浏览器默认的语言环境是中文时**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731511421500-651602c6-6dac-418c-9d1e-77c7ae40f463.png)



**测试2：将浏览器默认语言环境修改为法文**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731512808484-d61096e2-4d1d-446c-a26f-33c1505d76e4.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731512824927-f649b549-0add-4450-9dc3-9e2a4331c025.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 国际化实现原理
做国际化的自动配置类是：`MessageSourceAutoConfiguration`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731513242246-ac4e59e3-a1e9-45c3-a346-41ae1b41f658.png)

通过以上源码得知，国际化对应的配置前缀是：`spring.message`

例如在`application.properties`中进行如下配置：

```properties
# 配置国际化文件命名的基础名称
spring.messages.basename=messages

# 指定国际化信息的字符编码方式
spring.messages.encoding=UTF-8
```

注意：标准标识符：en_US 和 zh_CN 这样的标识符是固定的，不能更改。可以设置的是basename。



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

## 在程序当中如何获取国际化信息
在国际化自动配置类中可以看到这样一个Bean：MessageSource，它是专门用来处理国际化的。我们可以将它注入到我们的程序中，然后调用相关方法在程序中获取国际化信息。

```java
@Controller
public class MyController {

    @Autowired
    private MessageSource messageSource;

    @GetMapping("/test")
    @ResponseBody
    public String test(HttpServletRequest request){
        Locale locale = request.getLocale();
        String message = messageSource.getMessage("welcome.message", null, locale);
        return message;
    }
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

# 定制web容器
## web服务器切换为jetty
springboot默认嵌入的web服务器是Tomcat，如何切换到jetty服务器？

实现方式：排除Tomcat，添加Jetty依赖

**修改 **`pom.xml`** 文件**：在 `pom.xml` 中，确保你使用 `spring-boot-starter-web` 并排除 Tomcat，然后添加 Jetty 依赖。

```xml
<!-- 排除 Tomcat -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!-- 添加 Jetty 依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>

```

## web服务器切换原理
从哪里可以看出springboot是直接将tomcat服务器嵌入到应用中的呢？看这个类：`ServletWebServerFactoryAutoConfiguration`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731572949358-481582b6-0e79-4f4c-b556-b1ec3c13882c.png)

以上代码显示嵌入的是3个服务器。但并不是都生效，我们来看一下生效条件：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573044414-b1cb4767-de21-4897-9d38-36e6382a8581.png)

生效条件是，看类路径当中是否有对应服务器相关的类，如果有则生效。`spring-boot-web-starter`这个web启动器引入的时候，大家都知道，它间接引入的是tomcat服务器的jar包。因此默认Tomcat服务器被嵌入。如果想要切换web服务器，将tomcat相关jar包排除掉，引入jetty的jar包之后，jetty服务器就会生效，这就是切换web服务器的原理。

## web服务器优化
通过以下源码得知，web服务器的相关配置和`ServerProperties`有关系：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573299936-414ae5c8-87a8-4810-b448-ac1b67f3cfb2.png)

查看`ServerProperties`源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573337370-7d40853c-9c5a-4750-a40d-cf00d1528fb4.png)

得知web服务器的配置都是以`server`开头的。



那么如果要配置tomcat服务器怎么办？要配置jetty服务器怎么办？请看一下源码

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573416792-229c5dbb-4a42-4037-934d-bd004bc46a7d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573452454-a150e6b1-6fa0-411c-b2a5-063f5b03be6b.png)

通过以上源码得知，如果要对tomcat服务器进行配置，前缀为：`server.tomcat`

如果要对jetty服务器进行配置，前缀为：`server.jetty`。



在以后的开发中关于tomcat服务器的常见优化配置有：

```properties
# 这个参数决定了 Tomcat 在接收请求时，如果在指定的时间内没有收到完整的请求数据，将会关闭连接。这个超时时间是从客户端发送请求开始计算的。
# 防止长时间占用资源：如果客户端发送请求后，长时间没有发送完所有数据，Tomcat 会在这个超时时间内关闭连接，从而释放资源，避免资源被长时间占用。
server.tomcat.connection-timeout=20000

# 设置 Tomcat 服务器处理请求的最大线程数为 200。
# 如果超过这个数量的请求同时到达，Tomcat 会将多余的请求放入一个等待队列中。
# 如果等待队列也满了（由 server.tomcat.accept-count 配置），新的请求将被拒绝，通常会返回一个“503 Service Unavailable”错误。
server.tomcat.max-threads=200

# 用来设置等待队列的最大容量
server.tomcat.accept-count=100

# 设置 Tomcat 服务器在空闲时至少保持 10 个线程处于活动状态，以便快速响应新的请求。
server.tomcat.min-spare-threads=10

# 允许 Tomcat 服务器在关闭后立即重新绑定相同的地址和端口，即使该端口还在 TIME_WAIT 状态
# 当一个网络连接关闭时，操作系统会将该连接的端口保持在 TIME_WAIT 状态一段时间（通常是 2-4 分钟），以确保所有未完成的数据包都能被正确处理。在这段时间内，该端口不能被其他进程绑定。
server.tomcat.address-reuse-enabled=true

# 设置 Tomcat 服务器绑定到所有可用的网络接口，使其可以从任何网络地址访问。
server.tomcat.bind-address=0.0.0.0

# 设置 Tomcat 服务器使用 HTTP/1.1 协议处理请求。
server.tomcat.protocol=HTTP/1.1

# 设置 Tomcat 服务器的会话(session)超时时间为 30 分钟。具体来说，如果用户在 30 分钟内没有与应用进行任何交互，其会话将被自动注销。
server.tomcat.session-timeout=30

# 设置 Tomcat 服务器的静态资源缓存时间为 3600 秒（即 1 小时），这意味着浏览器会在 1 小时内缓存这些静态资源，减少重复请求。
server.tomcat.resource-cache-period=3600

# 解决get请求乱码。对请求行url进行编码。
server.tomcat.uri-encoding=UTF-8

# 设置 Tomcat 服务器的基础目录为当前工作目录（. 表示当前目录）。这个配置指定了 Tomcat 服务器的工作目录，包括日志文件、临时文件和其他运行时生成的文件的存放位置。 生产环境中可能需要重新配置。
server.tomcat.basedir=.
```

