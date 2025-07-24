**c命名空间是简化构造方法注入的。**  
使用c命名空间的两个前提条件：  
* **第一：需要在xml配置文件头部添加信息：xmlns:c="http://www.springframework.org/schema/c"**
* **第二：构造方法必须得存在。**
* **第三：使用方式**
	* **下标方式 ：简单类型——c:\_0=“属性值”；非简单类型——c:\_0-ref=“Bean的id”**
	* **参数名方式：c:name=“属性值”；非简单类型——c:\name-ref=“Bean的id”**
```java
package com.powernode.spring6.beans;

/**
 * @author 动力节点
 * @version 1.0
 * @className MyTime
 * @since 1.0
 **/
public class MyTime {
    private int year;
    private int month;
    private int day;

    public MyTime(int year, int month, int day) {
        this.year = year;
        this.month = month;
        this.day = day;
    }

    @Override
    public String toString() {
        return "MyTime{" +
                "year=" + year +
                ", month=" + month +
                ", day=" + day +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--<bean id="myTimeBean" class="com.powernode.spring6.beans.MyTime" c:year="1970" c:month="1" c:day="1"/>-->

<!--  
第一步：在spring的配置文件头部添加: xmlns:c="http://www.springframework.org/schema/c"  
第二步：使用  
c:_0 下标方式  
c:name 参数名方式  
-->
    <bean id="myTimeBean" class="com.powernode.spring6.beans.MyTime" c:_0="2008" c:_1="8" c:_2="8"/>

</beans>
```
```java
@Test
public void testC(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-c.xml");
    MyTime myTimeBean = applicationContext.getBean("myTimeBean", MyTime.class);
    System.out.println(myTimeBean);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665216171317-2dc02c42-3f3e-42f5-80e7-c578888e2fbb.png#averageHue=%2394856d&clientId=ufc7e21e2-2cbb-4&from=paste&height=118&id=ue696a583&originHeight=118&originWidth=487&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11561&status=done&style=shadow&taskId=ua699a0a9-fe16-4e30-9d2b-71d4a4b99ee&title=&width=487)
把构造方法注释掉：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665216339051-c5eecc20-6801-46dd-a331-e5b33c66c7ed.png#averageHue=%23352b2b&clientId=ufc7e21e2-2cbb-4&from=paste&height=216&id=u54a2f985&originHeight=216&originWidth=1176&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19996&status=done&style=shadow&taskId=u2436bd3d-d8e6-4a92-9b23-68bb0fcc84c&title=&width=1176)
所以，c命名空间是依靠构造方法的。
**注意：不管是p命名空间还是c命名空间，注入的时候都可以注入简单类型以及非简单类型。**