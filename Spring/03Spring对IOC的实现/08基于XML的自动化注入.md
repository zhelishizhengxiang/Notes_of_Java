Spring还可以完成自动化的注入，**自动化注入又被称为自动装配**。它可以根据**名字**进行自动装配，也可以根据**类型**进行自动装配。

**注意：不论是哪种自动装配，都是基于set注入实现的。更多的会使用到根据名字自动装配**
### 1.根据名称自动装配

```java
package com.powernode.spring6.dao;

/**
 * @author 动力节点
 * @version 1.0
 * @className UserDao
 * @since 1.0
 **/
public class UserDao {

    public void insert(){
        System.out.println("正在保存用户数据。");
    }
}
```

```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;

/**
 * @author 动力节点
 * @version 1.0
 * @className UserService
 * @since 1.0
 **/
public class UserService {

    private UserDao aaa;

    // 这个set方法非常关键
    public void setAaa(UserDao aaa) {
        this.aaa = aaa;
    }

    public void save(){
        aaa.insert();
    }
}

```
Spring的配置文件这样配置：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<!--根据名字进行自动装配-->  
	<!--注意：自动装配也是基于set方式实现的。-->
    <bean id="userService" class="com.powernode.spring6.service.UserService" autowire="byName"/>
    <!--id一般也叫作bean的名称。-->  
	<!--根据名字进行自动装配的时候，被注入的对象的bean的id不能随便写，怎么写？set方法的方法名去掉set，剩下单词首字母小写。-->
    <bean id="aaa" class="com.powernode.spring6.dao.UserDao"/>

</beans>
```
这个配置起到关键作用：  
- **UserService Bean中需要添加autowire="byName"，表示通过名称进行装配**。
- **根据名称装配(byName)，底层会调用set方法进行注入**。
- **根据名字进行自动装配的时候，被注入的对象的bean的id不能随便写，怎么写？set方法的方法名去掉set，剩下单词首字母小写**。
```java
@Test
public void testAutowireByName(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-autowire.xml");
    UserService userService = applicationContext.getBean("userService", UserService.class);
    userService.save();
}
```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665535913374-7031648f-fad4-4fa1-a3f1-68dcf2318bef.png#averageHue=%23f5f4f3&clientId=ubfe41891-11ea-4&from=paste&height=116&id=u09b0d555&originHeight=116&originWidth=471&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9778&status=done&style=shadow&taskId=u99de35e6-3c78-4628-b282-8fe94b88194&title=&width=471)

### 2.根据类型自动装配
**根据类型自动装配中的类型指的是数据类型**

```java
package com.powernode.spring6.dao;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountDao
 * @since 1.0
 **/
public class AccountDao {
    public void insert(){
        System.out.println("正在保存账户信息");
    }
}
```

```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.AccountDao;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountService
 * @since 1.0
 **/
public class AccountService {
    private AccountDao accountDao;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }

    public void save(){
        accountDao.insert();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--byType表示根据类型自动装配-->
    <!--自动装配是基于set方法的-->  
	<!--根据类型进行自动装配的时候，在有效的配置文件当中，某种类型的实例只能有一个。-->
    <bean id="accountService" class="com.powernode.spring6.service.AccountService" autowire="byType"/>

    <bean class="com.powernode.spring6.dao.AccountDao"/>

</beans>
```

```java
@Test
public void testAutowireByType(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-autowire.xml");
    AccountService accountService = applicationContext.getBean("accountService", AccountService.class);
    accountService.save();
}
```
执行结果： 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665537096983-d3c25b4c-21e1-499f-b348-6f829bc84a48.png#averageHue=%23f4f3f2&clientId=ubfe41891-11ea-4&from=paste&height=109&id=ucf231dcd&originHeight=109&originWidth=514&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9362&status=done&style=shadow&taskId=u73dc5c4e-c505-4247-8652-02ac58e7020&title=&width=514)
* **无论是byName还是byType，在装配的时候都是基于set方法的。所以set方法是必须要提供的。**
* **根据类型进行自动装配的时候，在有效的配置文件当中，某种类型的实例只能有一个。即配置文件中不能有两个类型一样的bean**。所以下例会报错
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountService" class="com.powernode.spring6.service.AccountService" autowire="byType"/>

    <bean id="x" class="com.powernode.spring6.dao.AccountDao"/>
    <bean id="y" class="com.powernode.spring6.dao.AccountDao"/>

</beans>
```
执行测试程序：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665537341888-57af14a1-eeb4-4070-8713-b4368003251d.png#averageHue=%23faf7f6&clientId=ubfe41891-11ea-4&from=paste&height=254&id=uee149cb5&originHeight=254&originWidth=1583&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57785&status=done&style=shadow&taskId=ud9ec74f0-3975-42b6-9535-c4c92b16535&title=&width=1583)
