
## 一、Servlet3.0新特性

* **Servlet3.0新特性：web.xml文件可以不写了。**
* **在Servlet3.0的时候，规范中提供了一个接口**：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711700341492-8c9a85d9-bca5-484f-8d5d-c3939f48db95.png#averageHue=%232f2b2a&clientId=ubb9f0330-cf20-4&from=paste&height=471&id=u141ebb8e&originHeight=471&originWidth=921&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69355&status=done&style=none&taskId=u4482458b-185d-486f-beaf-f1ff6ae5318&title=&width=921)
* **服务器在启动的时候会自动从容器中找 `ServletContainerInitializer`接口的实现类，自动调用它的`onStartup`方法来完成Servlet上下文的初始化**。

**在Spring3.1版本的时候，提供了这样一个类，实现以上的接口**：      
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711700544729-77092224-626d-4b76-8408-f3744fe2ad72.png#averageHue=%232e2b2a&clientId=ubb9f0330-cf20-4&from=paste&height=246&id=ua7872bf7&originHeight=246&originWidth=939&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37408&status=done&style=none&taskId=u055ee1ec-5e43-45e9-9b58-6d8d4d036ca&title=&width=939)

它的核心方法如下：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711700669446-3bcc469c-71d3-423a-86f7-52e95b73f344.png#averageHue=%232d2c2b&clientId=ubb9f0330-cf20-4&from=paste&height=631&id=uf4b1d92d&originHeight=631&originWidth=1149&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94594&status=done&style=none&taskId=u8bb242c1-8643-46a0-9d3f-94719c72818&title=&width=1149)

**可以看到在服务器启动的时候，他会自动调用``SpringServletContainerInitializer`的`onStartup`方法``，该方法中将`WebApplicationInitializer`会被加载，即它会去加载所有实现`WebApplicationInitializer`接口的类完成Servlet上下文的初始化**：      
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711700736674-05682c42-1904-4311-aede-b2e7994bfabf.png#averageHue=%232c2b2b&clientId=ubb9f0330-cf20-4&from=paste&height=424&id=ub1510e11&originHeight=424&originWidth=803&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53562&status=done&style=none&taskId=u3ad0df7d-2565-4197-978f-3a21fcbdc66&title=&width=803)


* **这个接口下有一个子类是我们需要的：`AbstractAnnotationConfigDispatcherServletInitializer`**
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711700804612-90b68082-5b55-4084-90fb-c230f6aed3a9.png#averageHue=%2376936b&clientId=ubb9f0330-cf20-4&from=paste&height=143&id=u1135318a&originHeight=143&originWidth=681&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19211&status=done&style=none&taskId=uafdd46a2-b2b1-4deb-825d-65ffb84d263&title=&width=681)
* ==**当我们编写类继承`AbstractAnnotationConfigDispatcherServletInitializer`之后，实现相应的方法，web服务器在启动的时候会根据它来初始化Servlet上下文**。==

![未命名文件.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711701535524-d2635ca6-3bae-4613-9dbb-ed6cb0b7dca6.png#averageHue=%2394d868&clientId=ubb9f0330-cf20-4&from=paste&height=619&id=uc2f1acbd&originHeight=619&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45462&status=done&style=shadow&taskId=u0b9a982a-b805-451a-b9cc-03d6b334875&title=&width=813)
* 写这个子类，就相当于是在写web.xml文件一样

## 二、编写WebAppInitializer

* 以下这个类就是用来代替web.xml文件的。
* **既然用该类来替代一个配置文件，就需要再该类上加@Configuration注解来表明该类是配置类**
* **下面的三个方法是继承父类必须要实现的三个方法，在其中写配置信息。其中`getRootConfigClasses()`是用于写spring当中配置信息的方法；`getServletConfigClasses()`是用于配置Spring MVC当中配置信息（即springmvc.xml）的方法；`getServletMappings()`方法适用于配置DispatcherServlet的url-pattern，也就是映射路径**
* **无论是Spring的配置方法，还是Spring MVC的配置方法，都是要返回一个类的Class对象。所以很明显我们需要单独写Spring和Spring MVC的配置类，之后在里面方法直接返回该类的Class对象即可**
* **web.xml中除了前端控制器要配置还需要配置字符编码过滤器等其他过滤器。重写`getServletFilters()`可以进行过滤器的配置，使用什么过滤器直接在该方法里面new出来设置好属性，最后返回过滤器数组即可。**
```java
package com.powernode.springmvc.config;

import jakarta.servlet.Filter;
import org.springframework.web.filter.CharacterEncodingFilter;
import org.springframework.web.filter.HiddenHttpMethodFilter;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

/**
 * ClassName: WebAppInitializer
 * Description:
 * Datetime: 2024/3/29 16:50
 * Author: 老杜@动力节点
 * Version: 1.0
 */
public class WebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * Spring的配置
     * @return
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    /**
     * SpringMVC的配置
     * @return
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMVCConfig.class};
    }

    /**
     * 用于配置 DispatcherServlet 的映射路径
     * @return
     */
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    /**
     * 配置过滤器
     * @return
     */
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceRequestEncoding(true);
        characterEncodingFilter.setForceResponseEncoding(true);
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
        return new Filter[]{characterEncodingFilter, hiddenHttpMethodFilter};
    }
}

```



Spring配置如下：
```java
package com.powernode.springmvc.config;

import org.springframework.context.annotation.Configuration;

/**
 * ClassName: SpringConfig
 * Description:
 * Datetime: 2024/3/29 17:03
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Configuration // 使用该注解指定这是一个配置类
public class SpringConfig {
}

```



SpringMVC配置如下：
```java
package com.powernode.springmvc.config;

import org.springframework.context.annotation.Configuration;

/**
 * ClassName: SpringMVCConfig
 * Description:
 * Datetime: 2024/3/29 17:03
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Configuration
public class SpringMVCConfig {
}

```

