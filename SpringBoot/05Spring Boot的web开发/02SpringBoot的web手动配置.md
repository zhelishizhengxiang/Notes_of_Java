
## 一、静态资源配置
如果你对SpringBoot默认的静态资源处理方式不满意。可以通过两种方式来改变这些默认的配置：

+ **第一种：配置文件方式**
    - **通过修改`application.properties`或`application.yml`**
    - **添加`spring.mvc`和`spring.web`相关的配置。**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730431752471-f806b5a3-924c-4b84-9c13-6999dad0493b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730431765824-5d3ec11d-aad5-44c8-a32e-dd255bec24ea.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730431775131-4bec5477-fd86-4ab2-afa1-4b1927b2aafc.png)

+ **第二种：编写代码方式**
    - **SpringMVC框架为我们提供了`WebMvcConfigurer`接口，需要改变默认的行为，可以`编写一个类`实现`WebMvcConfigurer`接口，并`对应重写`接口中的方法即可改变默认的配置行为**。



## 1.配置文件方式
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


访问webjars测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433063487-fe214e35-a43c-4ad0-9760-742f6bfcdcf6.png)



**访问普通静态资源测试结果如下：**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433000967-37e76b55-5715-4387-a135-5daacd53a755.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433110267-b2f84e91-6b95-48d3-88d6-acff3dca26cc.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433134598-5ac4583f-e7c1-4a8e-a3bb-77e2a3a33ac7.png)




如果访问`dog2.jpg`，就无法访问了：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433508569-ce398096-8cae-42e3-b6a2-5fe94e2a007a.png)



但是，存储在`classpath:/META-INF/resources/`目录下的`dog1.jpg`仍然是可以访问的：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730433537233-e7fe1330-22d3-4b36-8c43-bbdc9a578f99.png)

**因此，存储在`classpath:/META-INF/resources/`位置的静态资源会被默认加载，不受手动配置的影响。**





## 2.编写代码方式
我们在前面提到过，想要定制Spring MVC的行为，也可以编写类实现Spring MVC框架提供的一个接口`WebMvcConfigurer`，想定制哪个行为就重写哪个方法即可。

**编写的类只要纳入IoC容器的管理。因此有以下两种实现方式**：
+ **第一种：编写类实现`WebMvcConfigurer`接口，重写对应的方法**。
+ **第二种：以组件的形式存在：编写一个方法，用`@Bean`注解标注`WebMvcConfigurer`组件**。

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





启动服务器进行测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444208880-ca582b52-a68f-4918-9ead-f342c5ebbf38.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444226844-d1e40613-8c6c-43a5-95a1-1c0d34d101bb.png)

通过测试，我们的配置是生效的。

我们再来看看，默认的配置是否还生效？

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444289901-3304e315-67e2-4d6c-93b8-0db5f48a24a7.png)





我们可以看到，**Spring Boot对Spring MVC的默认自动配置是生效的。**

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





测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444971338-0aca22a6-ff00-4644-9e21-2d08765b62e4.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730444986018-554aeccd-a0fd-43d2-9b92-7846fea2415c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730445003557-89783ce1-fdf7-4b22-873b-0fafdc07ecd2.png)

通过了测试，并且以上代码也是在原有配置基础上进行扩展。



如果要弃用默认配置，仍然使用	`@EnableWebMvc`注解进行标注。自行测试！





### 其他配置也这样做即可
**以上对`静态资源处理`进行了手动配置，也可以做其他配置，例如拦截器**：

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




### 为什么只要容器中有`WebMvcConfigurer`组件即可呢
源码分析：



`WebMvcAutoConfiguration`部分源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730446108216-7ff9fb75-c9dd-4ba3-8b59-23f6c78d0b1a.png)

`WebMvcAutoConfiguration`类的内部类`EnableWebMvcConfiguration`，这个类继承了`DelegatingWebMvcConfiguration`（Delegating是委派的意思。）


`DelegatingWebMvcConfiguration`部分源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730446241532-6f81bf0c-4a9b-43a1-b816-12ccb6cfdeeb.png)

`DelegatingWebMvcConfiguration`中的`setConfigurers()`方法用来设置配置。而配置参数是一个List集合，这个List集合中存放的是`WebMvcConfigurer`接口的实例，并且可以看到这个方法上面使用了`@Autowired`进行了自动注入，这也就是说为什么只要是IoC容器中的组件就能生效的原因。

我们再次进入到`this.configurers.addWebMvcConfigurers(configurers);`方法中进一步查看源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730446847673-e2758f97-fcc9-4c72-bc9c-ea4463afe751.png)



对于`WebMvcConfigurerComposite`类的代码来说，它是一个非常典型的`**<font style="color:#DF2A3F;">组合模式</font>**`。

组合模式的关键点：

1. 组合多个 WebMvcConfigurer 实例：WebMvcConfigurerComposite 通过 delegates 列表组合了多个 WebMvcConfigurer 实例。
2. 统一接口：WebMvcConfigurerComposite 实现了 WebMvcConfigurer 接口，因此可以像一个单一的 WebMvcConfigurer 一样被使用。
3. 代理调用：在实现 WebMvcConfigurer 接口的方法时，WebMvcConfigurerComposite 会遍历 delegates 列表，调用每个 WebMvcConfigurer 实例的相应方法。

总结：WebMvcConfigurerComposite 主要采用了组合模式的思想，将多个 WebMvcConfigurer 实例组合在一起，形成一个整体。

注意：组合模式是GoF 23种设计模式中的结构型设计模式。
