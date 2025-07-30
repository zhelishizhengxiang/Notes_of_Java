## 7.6 注入自定义Date

我们前面说过，**java.util.Date在Spring中被当做简单类型**，简单类型在注入的时候可以直接使用value属性或value标签来完成。但我们之前已经测试过了，**对于Date类型来说，采用value属性或value标签赋值的时候，对日期字符串的格式要求非常严格，必须是这种格式的：Mon Oct 10 14:30:26 CST 2022。其他格式是不会被识别的，所以日常使用中都是把他当作的非简单类型来使用**。如以下代码：
```java
package com.powernode.spring6.bean;

import java.util.Date;

/**
 * @author 动力节点
 * @version 1.0
 * @className Student
 * @since 1.0
 **/
public class Student {
    private Date birth;

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    @Override
    public String toString() {
        return "Student{" +
                "birth=" + birth +
                '}';
    }
}
```

```xml
<bean id="studentBean" class="com.powernode.spring6.bean.Student">
  <property name="birth" value="Mon Oct 10 14:30:26 CST 2002"/>
</bean>
```

```java
@Test
public void testDate(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    Student studentBean = applicationContext.getBean("studentBean", Student.class);
    System.out.println(studentBean);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665383744481-75de8e77-ac4e-46b8-90dc-1cd5264f66f2.png#averageHue=%238c7a63&clientId=ue2397093-2e4b-4&from=paste&height=109&id=u5dcba6e9&originHeight=109&originWidth=529&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12763&status=done&style=shadow&taskId=u440ccfca-0f20-4c75-a9bb-e9bba89015d&title=&width=529)
如果把日期格式修改一下：
```xml
<bean id="studentBean" class="com.powernode.spring6.bean.Student">
  <property name="birth" value="2002-10-10"/>
</bean>
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665383871708-89cd2ac9-6d31-40fc-a4a8-27d27cd35ad2.png#averageHue=%232e2c2b&clientId=ue2397093-2e4b-4&from=paste&height=203&id=u2c539d66&originHeight=203&originWidth=1304&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11880&status=done&style=shadow&taskId=u582a716c-fbd7-4fb7-b582-483723b0b40&title=&width=1304)

* 这种情况下，我们就可以使用FactoryBean来完成这个骚操作。

编写DateFactoryBean实现FactoryBean接口：
```java
package com.powernode.spring6.bean;

import org.springframework.beans.factory.FactoryBean;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author 动力节点
 * @version 1.0
 * @className DateFactoryBean
 * @since 1.0
 **/
public class DateFactoryBean implements FactoryBean<Date> {

    // 定义属性接收日期字符串
    private String date;

    // 通过构造方法给日期字符串属性赋值
    public DateFactoryBean(String date) {
        this.date = date;
    }

    @Override
    public Date getObject() throws Exception {
	    //在创建日期对象前进行加工处理
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        //将字符串（`String`）解析为对应的Date 对象
        return sdf.parse(this.date);
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }
}

```
编写spring配置文件：
```xml
<!--注册工厂Bean本身-->
<bean id="dateBean" class="com.powernode.spring6.bean.DateFactoryBean">
  <constructor-arg name="date" value="1999-10-11"/>
</bean>

<bean id="studentBean" class="com.powernode.spring6.bean.Student">
  <property name="birth" ref="dateBean"/>
</bean>
```
执行测试程序：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665373835672-90312dd8-81e3-4d0a-8b2d-06f90a3e9203.png#averageHue=%23978971&clientId=ue2397093-2e4b-4&from=paste&height=103&id=ucbd70ed3&originHeight=103&originWidth=505&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13066&status=done&style=shadow&taskId=u92f4799b-47c9-4a27-a6b3-3853b3e75b2&title=&width=505)


