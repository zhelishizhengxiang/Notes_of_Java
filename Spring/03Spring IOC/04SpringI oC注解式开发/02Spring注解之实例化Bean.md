
## 一、声明Bean的注解
负责声明Bean的注解，常见的包括四个：  
- **@Component**
- **@Controller**
- **@Service**
- **@Repository**

源码如下：
```java
package com.powernode.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(value = {ElementType.TYPE})
@Retention(value = RetentionPolicy.RUNTIME)
public @interface Component {
    String value();
}

```
```java
package org.springframework.stereotype;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}

```
```java
package org.springframework.stereotype;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}

```
```java
package org.springframework.stereotype;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}

```
* 通过源码可以看到，**@Controller、@Service、@Repository这三个注解都是@Component注解的别名。** 也就是说：这四个注解的功能都一样。用哪个都可以。
* **@AliasFor(annotation = Component.class）代表该注解名字是Component注解的别名**
* 只是为了增强程序的可读性，建议：
    - **控制器类上使用：Controller**
    - **service类上使用：Service**
    - **dao类上使用：Repository**
* **他们都是只有一个value属性。value属性用来指定bean的id，也就是bean的名字。**   
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665545099269-ebd7e446-bc2f-4442-89b8-3f513e546a8b.png#averageHue=%23fcfbf9&clientId=u999e1312-db7a-4&from=paste&height=362&id=ue3348a0b&originHeight=362&originWidth=723&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37314&status=done&style=shadow&taskId=u1ccac9ac-0fa8-4792-bd2f-d433a8ffd3e&title=&width=723)


## 二、Spring注解的使用
如何使用以上的注解呢？  
- 第一步：加入aop的依赖
- 第二步：在配置文件中添加context命名空间
- 第三步：在配置文件中指定扫描的包
- 第四步：在Bean类上使用注解

**第一步：加入aop的依赖**  
我们可以看到当加入spring-context依赖之后，会关联加入aop的依赖。所以这一步不用做。  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665545268001-e3fb24f3-6688-4f52-a8c7-7c3084fa10a2.png#averageHue=%23faf8f5&clientId=u999e1312-db7a-4&from=paste&height=206&id=u99ab43ce&originHeight=206&originWidth=434&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14246&status=done&style=shadow&taskId=u9a6c4b51-5411-4887-aa32-aadd701f90c&title=&width=434)
**第二步：在配置文件中添加context命名空间**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
```
**第三步：在配置文件中指定要扫描的包**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--给Spring框架指定要扫描那些包中类-->                       
    <context:component-scan base-package="com.powernode.spring6.bean"/>
</beans>
```
**第四步：在Bean类上使用注解**
```java
package com.powernode.spring6.bean;

import org.springframework.stereotype.Component;

@Component(value = "userBean")
public class User {
}

```
编写测试程序：
```java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.User;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationTest {
    @Test
    public void testBean(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        User userBean = applicationContext.getBean("userBean", User.class);
        System.out.println(userBean);
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665545669944-c067eacb-f65b-45ab-b68b-2320647cdfb4.png#averageHue=%23f3f2f0&clientId=u999e1312-db7a-4&from=paste&height=114&id=u818265a9&originHeight=114&originWidth=537&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12818&status=done&style=shadow&taskId=ub07ca18c-5947-43fc-958f-78164acfa8f&title=&width=537)

**如果注解的属性名是value，那么value是可以省略的。**
```java
package com.powernode.spring6.bean;

import org.springframework.stereotype.Component;

@Component("vipBean")
public class Vip {
}
```


```java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.Vip;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationTest {
    @Test
    public void testBean(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Vip vipBean = applicationContext.getBean("vipBean", Vip.class);
        System.out.println(vipBean);
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665545860738-8bae2a45-efa8-40eb-9213-0dbd2ae1b54a.png#averageHue=%23f1efed&clientId=u999e1312-db7a-4&from=paste&height=107&id=u18d31f9a&originHeight=107&originWidth=496&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12984&status=done&style=shadow&taskId=uaf671349-651d-42ec-b4c6-d49ae4538dd&title=&width=496)

==**如果把value属性彻底去掉，spring会被Bean自动取名吗？会的。并且默认名字的规律是：Bean类名首字母小写即可。**==
```java
package com.powernode.spring6.bean;

import org.springframework.stereotype.Component;

@Component
public class BankDao {
}
```
也就是说，这个BankDao的bean的名字为：bankDao
```java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.BankDao;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationTest {
    @Test
    public void testBean(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        BankDao bankDao = applicationContext.getBean("bankDao", BankDao.class);
        System.out.println(bankDao);
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665546100844-e0ffc213-8126-419a-ab67-7f433ad43105.png#averageHue=%23f2f0ee&clientId=u999e1312-db7a-4&from=paste&height=110&id=u1c31bf25&originHeight=110&originWidth=540&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13208&status=done&style=shadow&taskId=u9d91d502-d4e0-47a5-9253-31562560c5f&title=&width=540)

我们将Component注解换成其它三个注解，看看是否可以用：
```java
package com.powernode.spring6.bean;

import org.springframework.stereotype.Controller;

@Controller
public class BankDao {
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665546198246-f9d6adc1-ecc8-4e8c-babf-49f2ed7b87cd.png#averageHue=%23f2f0ee&clientId=u999e1312-db7a-4&from=paste&height=109&id=u34fc7e45&originHeight=109&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13171&status=done&style=shadow&taskId=u211caeee-724d-410c-9440-f1136f928e4&title=&width=507)

剩下的两个注解大家可以测试一下。

**如果是多个包怎么办？有两种解决方案：**    
- **第一种：在配置文件中指定多个包，用逗号隔开。**
- **第二种：指定多个包的共同父包。但是一定会牺牲一部分效率，因为扫描范围更大**

先来测试一下逗号（英文）的方式：
创建一个新的包：bean2，定义一个Bean类。
```java
package com.powernode.spring6.bean2;

import org.springframework.stereotype.Service;

@Service
public class Order {
}

```
配置文件修改：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.powernode.spring6.bean,com.powernode.spring6.bean2"/>
</beans>
```
测试程序：
```java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.BankDao;
import com.powernode.spring6.bean2.Order;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationTest {
    @Test
    public void testBean(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        BankDao bankDao = applicationContext.getBean("bankDao", BankDao.class);
        System.out.println(bankDao);
        Order order = applicationContext.getBean("order", Order.class);
        System.out.println(order);
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665546710304-8ebbe95d-1d1d-44fa-9605-9dad43e487b7.png#averageHue=%23f3f1ee&clientId=u999e1312-db7a-4&from=paste&height=136&id=uef2bb625&originHeight=136&originWidth=533&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19061&status=done&style=shadow&taskId=ua5845489-0baf-4d46-9312-549c3f66211&title=&width=533)

我们再来看看，指定共同的父包行不行：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.powernode.spring6"/>
</beans>
```
执行测试程序：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665546777022-4eb8c5e3-22ed-4baf-8722-a5fa98df253d.png#averageHue=%23f3f1ee&clientId=u999e1312-db7a-4&from=paste&height=142&id=u1924e1f2&originHeight=142&originWidth=519&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19344&status=done&style=shadow&taskId=ua561508b-6f89-463d-b7c2-ace912fb097&title=&width=519)


## 三、选择性实例化Bean
假设在某个包下有很多Bean，有的Bean上标注了Component，有的标注了Controller，有的标注了Service，有的标注了Repository，**现在由于某种特殊业务的需要，只允许其中所有的Controller参与Bean管理，其他的都不实例化。这应该怎么办呢？**

##### 第一种解决方案
```java
package com.powernode.spring6.bean3;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

@Component
public class A {
    public A() {
        System.out.println("A的无参数构造方法执行");
    }
}

@Controller
class B {
    public B() {
        System.out.println("B的无参数构造方法执行");
    }
}

@Service
class C {
    public C() {
        System.out.println("C的无参数构造方法执行");
    }
}

@Repository
class D {
    public D() {
        System.out.println("D的无参数构造方法执行");
    }
}

@Controller
class E {
    public E() {
        System.out.println("E的无参数构造方法执行");
    }
}

@Controller
class F {
    public F() {
        System.out.println("F的无参数构造方法执行");
    }
}

```
我只想实例化bean3包下的Controller。配置文件这样写：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.powernode.spring6.bean3" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <!--如果只包含两个就在下面多写一行-->
        <!--<context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>-->
    </context:component-scan>
    
</beans>
```
* **use-default-filters="true"为默认值。 表示：使用spring默认的规则，即只要有Component、Controller、Service、Repository中的任意一个注解标注，则进行实例化**。
* **use-default-filters="false" 表示：不再spring默认实例化规则，即使有Component、Controller、Service、Repository这些注解标注，也不再实例化。**
* **<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/> 表示只有写明了Controller注解的Bean会进行实例化。其中type属性指定类型，一般为annotation;expression写对应注解的全限定类名**
```java
@Test
public void testChoose(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-choose.xml");
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665556059297-de0d7dbc-aa37-46a3-9b1d-1d4c246b0ffc.png#averageHue=%23f7f6f4&clientId=u999e1312-db7a-4&from=paste&height=174&id=u46e6828e&originHeight=174&originWidth=520&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18046&status=done&style=shadow&taskId=uef3c6593-409c-4fbe-b4c5-9105a4c73d6&title=&width=80)

##### 第二种解决方案

**也可以将use-default-filters设置为true（不写就是true），并且采用exclude-filter方式排除哪些注解标注的Bean不参与实例化：**
```xml
<context:component-scan base-package="com.powernode.spring6.bean3">
  <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
  <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
  <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```
执行测试程序：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665556372417-14f2208c-4151-4bcd-9f22-80db5e3ed837.png#averageHue=%23f4f3f1&clientId=u999e1312-db7a-4&from=paste&height=111&id=u1ebe1eec&originHeight=111&originWidth=492&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11122&status=done&style=shadow&taskId=ud5a1de4a-95e7-459f-b1b0-a45af1307ff&title=&width=492)

