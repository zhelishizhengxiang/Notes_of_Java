**使用util命名空间可以让配置复用**。比如系统要去继承多个厂家的数据源，**连续统一数据库使用不同的数据源去做，配置信息写的都一模一样，那么就可以定义一个类似于工具类的配置信息**，然后进行复用

使用方法：
1. **使用util命名空间的前提是：在spring配置文件头部添加配置信息。如下**：  
	![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665218059794-30411b76-a22c-4339-ab60-acad8f02ab28.png#averageHue=%23312a2a&clientId=ufc7e21e2-2cbb-4&from=paste&height=212&id=udeece73c&originHeight=212&originWidth=1494&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44224&status=done&style=shadow&taskId=u39d74a7a-b50e-4d8e-b74b-c63406de34f&title=&width=1494)
2. **使用`util:property`标签将配置信息，指定id属性**
3. **在其他Bean的需要书写相同配置信息的property属性中，使用ref标签去传入uitl配置信息的id**
```java
package com.powernode.spring6.beans;

import java.util.Properties;

/**
 * @author 动力节点
 * @version 1.0
 * @className MyDataSource1
 * @since 1.0
 **/
public class MyDataSource1 {
    private Properties properties;

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    @Override
    public String toString() {
        return "MyDataSource1{" +
                "properties=" + properties +
                '}';
    }
}
```

```java
package com.powernode.spring6.beans;

import java.util.Properties;

/**
 * @author 动力节点
 * @version 1.0
 * @className MyDataSource2
 * @since 1.0
 **/
public class MyDataSource2 {
    private Properties properties;

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    @Override
    public String toString() {
        return "MyDataSource2{" +
                "properties=" + properties +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <!--引入util命名空间  
	在spring的配置文件头部添加：  
	xmlns:util="http://www.springframework.org/schema/util"  
	  
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd  
	-->  
	<util:properties id="prop">  
		<prop key="driver">com.mysql.cj.jdbc.Driver</prop>  
		<prop key="url">jdbc:mysql://localhost:3306/spring6</prop>  
		<prop key="username">root</prop>  
		<prop key="password">123</prop>  
	</util:properties>  
	  
	<!--数据源1-->  
	<bean id="ds1" class="com.powernode.spring6.jdbc.MyDataSource1">  
		<property name="properties" ref="prop"/>  
	</bean>  
	  
	<!--数据源2-->  
	<bean id="ds2" class="com.powernode.spring6.jdbc.MyDataSource2">  
		<property name="properties" ref="prop"/>  
	</bean>
</beans>
```

```java
@Test
public void testUtil(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-util.xml");

    MyDataSource1 dataSource1 = applicationContext.getBean("dataSource1", MyDataSource1.class);
    System.out.println(dataSource1);

    MyDataSource2 dataSource2 = applicationContext.getBean("dataSource2", MyDataSource2.class);
    System.out.println(dataSource2);
}
```
执行结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665218430727-c81e399e-294e-4bb5-b98b-2c8875b0884f.png#averageHue=%23968168&clientId=ufc7e21e2-2cbb-4&from=paste&height=140&id=ud2836a05&originHeight=140&originWidth=1518&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29754&status=done&style=shadow&taskId=ubb9d8e9e-3a21-4a14-8fe5-ff81df06522&title=&width=1518)
