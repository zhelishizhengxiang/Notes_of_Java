**目的：简化配置。**    
使用p命名空间注入的前提条件包括两个：  
- **第一：在XML头部信息中添加p命名空间的配置信息：xmlns:p="http://www.springframework.org/schema/p**
- **第二：p命名空间注入是基于setter方法的，所以需要对应的属性提供setter方法。**
- **第三：简单类型使用 p:属性名 = "属性值" ；非简单类型使用p:属性名-ref = "Bean的id"。就不必用property属性了**（前提是符合JavaBean规范，所以才是属性名）
```java
package com.powernode.spring6.beans;

/**
 * @author 动力节点
 * @version 1.0
 * @className Customer
 * @since 1.0
 **/
public class Dog {  
	// 简单类型  
	private String name;  
	private int age;  
	// 非简单类型  
	private Date birth;  
	  
	// p命名空间注入底层还是set注入，只不过p命名空间注入可以让spring配置变的更加简单。  
	public void setName(String name) {  
		this.name = name;  
	}  
	  
	public void setAge(int age) {  
		this.age = age;  
	}  
	  
	public void setBirth(Date birth) {  
		this.birth = birth;  
	}  
	  
	@Override  
	public String toString() {  
		return "Dog{" +  
		"name='" + name + '\'' +  
		", age=" + age +  
		", birth=" + birth +  
		'}';  
	}  
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
	   xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--在XML头部信息中添加p命名空间的配置信息--> 

<!--  
第一步：在spring的配置文件头部添加p命名空间。xmlns:p="http://www.springframework.org/schema/p"  
第二步：使用 p:属性名 = "属性值" ,就不必用property
-->
    <bean id="dogBean" class="com.powernode.spring6.bean.Dog" p:name="小花" p:age="3" p:birth-ref="birthBean"/>  
  
	<!--这里获取的是当前系统时间。-->  
	<bean id="birthBean" class="java.util.Date"/>

</beans>
```

```java
@Test
public void testP(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-p.xml");
    Customer customerBean = applicationContext.getBean("customerBean", Customer.class);
    System.out.println(customerBean);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665215638858-c5ae8aef-43ec-455d-90a3-ac3f97c92746.png#averageHue=%238d7c66&clientId=ufc7e21e2-2cbb-4&from=paste&height=118&id=u4aeacd2a&originHeight=118&originWidth=473&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11448&status=done&style=shadow&taskId=u08f3e033-d49d-44e1-b717-e751097bdec&title=&width=473)
把setter方法去掉：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665215713205-fcebda06-c4bb-486b-a2d7-6a238088625b.png#averageHue=%23352c2b&clientId=ufc7e21e2-2cbb-4&from=paste&height=220&id=uf42f4afe&originHeight=220&originWidth=1058&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19291&status=done&style=shadow&taskId=u9c7f0649-555f-48d3-816e-a105727b293&title=&width=1058)
所以**p命名空间实际上是对set注入的简化。**