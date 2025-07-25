## 一、什么是Bean的循环依赖
A**对象中有B属性。B对象中有A属性。这就是循环依赖**。我依赖你，你也依赖我。

比如：丈夫类Husband，妻子类Wife。Husband中有Wife的引用。Wife中有Husband的引用。 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665452274046-82594b87-2974-4e08-a6ab-2218d001d14f.png#averageHue=%23f5f5f5&clientId=ue12e8566-378b-4&from=paste&height=162&id=u6d2f623e&originHeight=162&originWidth=347&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4376&status=done&style=shadow&taskId=uaa31889d-a21b-4589-a039-281453ab56a&title=&width=347)
```java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Husband
 * @since 1.0
 **/
public class Husband {
    private String name;
    private Wife wife;
}

```
```java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Wife
 * @since 1.0
 **/
public class Wife {
    private String name;
    private Husband husband;
}

```

## 二、 singleton下的set注入产生的循环依赖
我们来编写程序，测试一下在singleton+setter的模式下产生的循环依赖，Spring是否能够解决？
```java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Husband
 * @since 1.0
 **/
public class Husband {
    private String name;
    private Wife wife;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setWife(Wife wife) {
        this.wife = wife;
    }

    // toString()方法重写时需要注意：不能直接输出wife，输出wife.getName()。要不然会出现递归导致的栈内存溢出错误。
    @Override
    public String toString() {
        return "Husband{" +
                "name='" + name + '\'' +
                ", wife=" + wife.getName() +
                '}';
    }
}
```

```java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Wife
 * @since 1.0
 **/
public class Wife {
    private String name;
    private Husband husband;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setHusband(Husband husband) {
        this.husband = husband;
    }

    // toString()方法重写时需要注意：不能直接输出husband，输出husband.getName()。要不然会出现递归导致的栈内存溢出错误。
    @Override
    public String toString() {
        return "Wife{" +
                "name='" + name + '\'' +
                ", husband=" + husband.getName() +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<!--singleton + setter模式注入-->
    <bean id="husbandBean" class="com.powernode.spring6.bean.Husband" scope="singleton">
        <property name="name" value="张三"/>
        <property name="wife" ref="wifeBean"/>
    </bean>
    <bean id="wifeBean" class="com.powernode.spring6.bean.Wife" scope="singleton">
        <property name="name" value="小花"/>
        <property name="husband" ref="husbandBean"/>
    </bean>
</beans>
```

```java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.Husband;
import com.powernode.spring6.bean.Wife;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author 动力节点
 * @version 1.0
 * @className CircularDependencyTest
 * @since 1.0
 **/
public class CircularDependencyTest {

    @Test
    public void testSingletonAndSet(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Husband husbandBean = applicationContext.getBean("husbandBean", Husband.class);
        Wife wifeBean = applicationContext.getBean("wifeBean", Wife.class);
        System.out.println(husbandBean);
        System.out.println(wifeBean);
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665453201014-160bb88e-08d4-4d37-a1d9-44d4911a32df.png#averageHue=%238b7760&clientId=ue12e8566-378b-4&from=paste&height=149&id=u5b4b34dd&originHeight=149&originWidth=516&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16412&status=done&style=shadow&taskId=uf18aa1ed-430e-4ab5-9523-c0b0e54ba30&title=&width=516)
**通过测试得知：在singleton + set注入的情况下，循环依赖是没有问题的。Spring可以解决这个问题。**

在singleton + setter模式下，为什么循环依赖不会出现问题，Spring是如何应对的？   
* **主要的原因是：在这种模式下Spring对Bean的管理主要分为清晰的两个阶段**：  
	1. 在**Spring容器加载的时候，实例化Bean，只要其中任意一个Bean实例化之后，马上进行 “曝光”【不等属性赋值就曝光】**  
	2. **Bean“曝光”之后，再进行属性的赋值(调用set方法)**。
* **曝光的意思是当Bean 完成实例化（内存分配、构造器执行）但尚未完成属性注入和初始化时，就将其提前暴露到缓存中。这样，当其他 Bean 依赖它时，可以直接从缓存中获取这个 “半成品” Bean，而无需等待它完全初始化**
* **总结来说核心解决方案是：实例化对象和对象的属性赋值分为两个阶段来完成的**。  
  
注意：只有在scope是singleton的情况下，Bean才会采取提前“曝光”的措施。


## 三、prototype下的set注入产生的循环依赖
我们再来测试一下：prototype+set注入的方式下，循环依赖会不会出现问题？
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="husbandBean" class="com.powernode.spring6.bean.Husband" scope="prototype">
        <property name="name" value="张三"/>
        <property name="wife" ref="wifeBean"/>
    </bean>
    <bean id="wifeBean" class="com.powernode.spring6.bean.Wife" scope="prototype">
        <property name="name" value="小花"/>
        <property name="husband" ref="husbandBean"/>
    </bean>
</beans>
```
执行测试程序：发生了异常，异常信息如下：  
Caused by: org.springframework.beans.factory.**BeanCurrentlyInCreationException**: Error creating bean with name 'husbandBean': Requested bean is currently in creation: Is there an unresolvable circular reference?
	
翻译为：创建名为“husbandBean”的bean时出错：请求的bean当前正在创建中：是否存在无法解析的循环引用？
通过测试得知，当循环依赖的**所有Bean**的scope="prototype"的时候，产生的循环依赖，Spring是无法解决的，会出现**BeanCurrentlyInCreationException**异常。
大家可以测试一下，以上两个Bean，如果其中一个是singleton，另一个是prototype，是没有问题的。
为什么两个Bean都是prototype时会出错呢？
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665454469042-69668f45-5d71-494f-8537-18142d354abd.png#averageHue=%232f2c2b&clientId=ue12e8566-378b-4&from=paste&height=480&id=u51bd1a99&originHeight=480&originWidth=1140&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92057&status=done&style=shadow&taskId=u3b8b4f66-7f8c-4735-ac25-5aba78db2d5&title=&width=1140)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=DKByo&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)