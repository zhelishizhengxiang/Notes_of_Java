## 5.1 单例singleton

**默认情况下，Spring的IoC容器创建的Bean对象是单例的**。来测试一下：
```java
package com.powernode.spring6.beans;

/**
 * @author 动力节点
 * @version 1.0
 * @className SpringBean
 * @since 1.0
 **/
public class SpringBean {
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="sb" class="com.powernode.spring6.beans.SpringBean" />
    
</beans>
```
```java
@Test
public void testScope(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-scope.xml");

    SpringBean sb1 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb1);

    SpringBean sb2 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb2);
}
```

执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665220014331-a1e4cac5-c749-4b67-bab8-43d6957c35e4.png#averageHue=%23927d67&clientId=ufc7e21e2-2cbb-4&from=paste&height=142&id=u054fef63&originHeight=142&originWidth=599&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20728&status=done&style=shadow&taskId=ufb3a6c56-51b9-43f5-803d-c4ca68308aa&title=&width=599)
通过测试得知：Spring的IoC容器中，默认情况下，Bean对象是单例的。  

这个对象在什么时候创建的呢？可以为SpringBean提供一个无参数构造方法，测试一下，如下：
```java
package com.powernode.spring6.beans;

/**
 * @author 动力节点
 * @version 1.0
 * @className SpringBean
 * @since 1.0
 **/
public class SpringBean {
    public SpringBean() {
        System.out.println("SpringBean的无参数构造方法执行。");
    }
}

```
将测试程序中getBean()所在行代码注释掉：
```java
@Test
public void testScope(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-scope.xml");
}
```
执行测试程序：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665220250081-036fe814-8328-4b58-adf4-b3a37eb0cb4d.png#averageHue=%238e7f66&clientId=ufc7e21e2-2cbb-4&from=paste&height=114&id=ube1053bf&originHeight=114&originWidth=482&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12907&status=done&style=shadow&taskId=u22871dcd-91c0-46b1-b30e-3d03327e8c6&title=&width=482)
通过测试得知，默认情况下，Bean对象的创建是在初始化Spring上下文的时候就完成的。



## 5.2 多例prototype
**如果想让Spring的Bean对象以多例的形式存在，可以在bean标签中指定scope属性的值为：**prototype**，这样Spring会在每一次执行getBean()方法的时候创建Bean对象，调用几次则创建几次。**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!--
		第一个值：singleton 单例（默认值）  
		第二个值：prototype 原型/多例
	-->
    <bean id="sb" class="com.powernode.spring6.beans.SpringBean" scope="prototype" />

</beans>
```
```java
@Test
public void testScope(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-scope.xml");

    SpringBean sb1 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb1);

    SpringBean sb2 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb2);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665220593171-19b1a750-551c-441d-8f8f-9c7aa7601e77.png#averageHue=%23927c63&clientId=ufc7e21e2-2cbb-4&from=paste&height=198&id=udb383895&originHeight=198&originWidth=616&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32800&status=done&style=shadow&taskId=u3dea7c4e-0cc4-4a5e-ad06-76207e442c6&title=&width=616)
我们可以把测试代码中的getBean()方法所在行代码注释掉：
```java
@Test
public void testScope(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-scope.xml");
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665220698959-ff4ad909-670e-4745-960d-5c215e2af71e.png#averageHue=%238d7b65&clientId=ufc7e21e2-2cbb-4&from=paste&height=153&id=u098e4067&originHeight=153&originWidth=541&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10979&status=done&style=shadow&taskId=u2c63f077-00be-49c9-a310-71c3fc2aa7b&title=&width=541)

可以看到这一次在初始化Spring上下文的时候，并没有创建Bean对象。
那你可能会问：scope如果没有配置，**它的默认值是什么呢？默认值是singleton，单例的。**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="sb" class="com.powernode.spring6.beans.SpringBean" scope="singleton" />

</beans>
```
```java
@Test
public void testScope(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-scope.xml");

    SpringBean sb1 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb1);

    SpringBean sb2 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb2);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665221074412-9b48c6e3-44f0-492c-a712-37b4baa24341.png#averageHue=%23937e66&clientId=ufc7e21e2-2cbb-4&from=paste&height=173&id=u956152e2&originHeight=173&originWidth=573&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26951&status=done&style=shadow&taskId=uad922a99-49bc-4c58-a11e-9d2b89f6856&title=&width=573)
通过测试得知，没有指定scope属性时，默认是singleton单例的。

## 3.scope属性其他值
**scope属性的值不止两个，它一共包括8个选项：**

- singleton：默认的，单例。
- prototype：原型。每调用一次getBean()方法则获取一个新的Bean对象。或每次注入的时候都是新对象。
- **request：一个请求对应一个Bean。**仅限于在WEB应用中使用**。
- **session：一个会话对应一个Bean。**仅限于在WEB应用中使用**。（**request session要求项目必须是一个web应用。当你引入springmvc框架的时候，这两个值就可以使用了。**）
- global session：**portlet应用中专用的**。如果在Servlet的WEB应用中使用global session的话，和session一个效果。（portlet和servlet都是规范。servlet运行在servlet容器中，例如Tomcat。portlet运行在portlet容器中。）
- application：**一个应用对应一个Bean。仅限于在WEB应用中使用。**
- websocket：一个websocket生命周期对应一个Bean。**仅限于在WEB应用中使用。**（websocket是一种服务端像客户端实时推送内容的技术）
- 自定义scope：很少使用。


接下来咱们自定义一个Scope，线程级别的Scope，在同一个线程中，获取的Bean都是同一个。跨线程则是不同的对象：（以下内容作为了解）

- 第一步：自定义Scope。（实现Scope接口）
   - spring内置了线程范围的类：org.springframework.context.support.SimpleThreadScope，可以直接用。（这里使用内置的类，正常自定义scope需要写一个类实现Scope接口，然后将其注册）
- 第二步：将自定义的Scope注册到Spring容器中。
```xml
<bean class="org.springframework.beans.factory.config.CustomScopeConfigurer">
  <property name="scopes">
    <map>
      <entry key="myThread">
        <bean class="org.springframework.context.support.SimpleThreadScope"/>
      </entry>
    </map>
  </property>
</bean>
```

- 第三步：使用Scope。
```xml
<bean id="sb" class="com.powernode.spring6.beans.SpringBean" scope="myThread" />
```
编写测试程序：
```java
@Test
public void testCustomScope(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-scope.xml");
    SpringBean sb1 = applicationContext.getBean("sb", SpringBean.class);
    SpringBean sb2 = applicationContext.getBean("sb", SpringBean.class);
    System.out.println(sb1);
    System.out.println(sb2);
    // 启动线程
    new Thread(new Runnable() {
        @Override
        public void run() {
            SpringBean a = applicationContext.getBean("sb", SpringBean.class);
            SpringBean b = applicationContext.getBean("sb", SpringBean.class);
            System.out.println(a);
            System.out.println(b);
        }
    }).start();
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665297614749-ae92b97d-85fd-4792-af87-e72d35784187.png#averageHue=%23343331&clientId=u3fe1442a-4567-4&from=paste&height=260&id=u600551ec&originHeight=260&originWidth=587&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43796&status=done&style=shadow&taskId=u14c4b472-78e4-43c6-b593-93b39a42d3f&title=&width=587)

## 总结

 1. **默认情况下，Spring的IoC容器创建的Bean对象是单例的。**在Spring上下文初始化的时候实例化。  每一次调用getBean()方法的时候，都返回那个单例的对象。
 2. **当将bean的scope属性设置为prototype： bean是多例的。 spring上下文初始化的时候，并不会初始化这些prototype的bean。 每一次调用getBean()方法的时候，实例化该bean对象。**  
3. prototype翻译为：原型。**scope属性的默认值是singleton，单例的**。
4. **scope属性的值不止两个，它一共包括8个选项：**
	- singleton：默认的，单例。
	- prototype：原型。每调用一次getBean()方法则获取一个新的Bean对象。或每次注入的时候都是新对象。
	- **request：一个请求对应一个Bean。**仅限于在WEB应用中使用**。
	- **session：一个会话对应一个Bean。**仅限于在WEB应用中使用**。（**request session要求项目必须是一个web应用。当你引入springmvc框架的时候，这两个值就可以使用了。**）
	- global session：**portlet应用中专用的**。如果在Servlet的WEB应用中使用global session的话，和session一个效果。（portlet和servlet都是规范。servlet运行在servlet容器中，例如Tomcat。portlet运行在portlet容器中。）
	- application：**一个应用对应一个Bean。仅限于在WEB应用中使用。**
	- websocket：一个websocket生命周期对应一个Bean。**仅限于在WEB应用中使用。** （websocket是一种服务端像客户端实时推送内容的技术）
	- 自定义scope：由于很少使用此处不总结做法。

