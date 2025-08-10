![](assets/02Spring%20MVC的注解配置/file-20250809183855670.png)
## 组件扫描——@ComponentScan
```java
// 指定该类是一个配置类，可以当配置文件使用
@Configuration
// 开启组件扫描
@ComponentScan("com.powernode.springmvc.controller")
public class SpringMVCConfig {
}
```


## 开启注解驱动——@EnableWebMvc
```java
// 指定该类是一个配置类，可以当配置文件使用
@Configuration
// 开启组件扫描
@ComponentScan("com.powernode.springmvc.controller")
// 开启注解驱动
@EnableWebMvc
public class SpringMVCConfig {
}
```


## 视图解析器

具体的配置方法如下图所示    
![](assets/02Spring%20MVC的注解配置/file-20250809184455136.png)
```java
// 指定该类是一个配置类，可以当配置文件使用
@Configuration
// 开启组件扫描
@ComponentScan("com.powernode.springmvc.controller")
// 开启注解驱动
@EnableWebMvc
public class SpringMVCConfig {

    @Bean
    public ThymeleafViewResolver getViewResolver(SpringTemplateEngine springTemplateEngine) {
        ThymeleafViewResolver resolver = new ThymeleafViewResolver();
        resolver.setTemplateEngine(springTemplateEngine);
        resolver.setCharacterEncoding("UTF-8");
        resolver.setOrder(1);
        return resolver;
    }

    @Bean
    public SpringTemplateEngine templateEngine(ITemplateResolver iTemplateResolver) {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(iTemplateResolver);
        return templateEngine;
    }

    @Bean
    public ITemplateResolver templateResolver(ApplicationContext applicationContext) {
        SpringResourceTemplateResolver resolver = new SpringResourceTemplateResolver();
        resolver.setApplicationContext(applicationContext);
        resolver.setPrefix("/WEB-INF/thymeleaf/");
        resolver.setSuffix(".html");
        resolver.setTemplateMode(TemplateMode.HTML);
        resolver.setCharacterEncoding("UTF-8");
        resolver.setCacheable(false);//开发时关闭缓存，改动即可生效
        return resolver;
    }
}
```


## 开启默认Servlet处理

**让SpringMVCConfig类实现这个接口：`WebMvcConfigurer`，并且重写以下的方法：**
```java
@Override
public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
    configurer.enable();
}
```

## view-controller
**重写以下方法。同样需要让SpringMVCConfig类实现这个接口：`WebMvcConfigurer`**
```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/test").setViewName("test");
}
```


## 异常处理器
**重写以下方法：同样需要让SpringMVCConfig类实现这个接口：`WebMvcConfigurer`**
```java
@Override
public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
    SimpleMappingExceptionResolver resolver = new SimpleMappingExceptionResolver();
    //可以配置多个异常处理器
    /*  
	<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">  
	<property name="exceptionMappings">  
	<props>  
	<prop key="java.lang.Exception">tip</prop>  
	</props>  
	</property>  
	<property name="exceptionAttribute" value="yiChang"/>  
	</bean>  
	*/
    Properties prop = new Properties();
    prop.setProperty("java.lang.Exception", "tip");
    resolver.setExceptionMappings(prop);
    resolver.setExceptionAttribute("yiChang");
    resolvers.add(resolver);
}
```


## 拦截器
**重写以下方法：同样需要让SpringMVCConfig类实现这个接口：`WebMvcConfigurer
```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    MyInterceptor myInterceptor = new MyInterceptor();
	    registry.addInterceptor(myInterceptor).addPathPatterns("/**").excludePathPatterns("/test");
}
```



