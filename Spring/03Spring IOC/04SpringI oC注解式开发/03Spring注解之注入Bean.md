## 一、负责注入的注解
@Component @Controller @Service @Repository 这四个注解是用来声明Bean的，声明后这些Bean将被实例化。接下来我们看一下，如何给Bean的属性赋值。**给Bean属性赋值**需要用到这些注解：

- **@Value：注入简单类型**
- **@Autowired：注入非简单类型，单独使用@Autowired注解，默认根据类型装配**
- **@Qualifier：想要根据名字进行装配，将该注解配合与@Qualifier一起使用**
- **@Resource：默认根据名称装配byName，通过name属性来指定Bean的名称。未指定name时，使用属性名作为name。通过name找不到的话会自动启动通过类型byType装配。**
### 1.@Value
@Value注解的源代码如下图所示  
![](assets/03Spring注解之注入Bean/file-20250727163836134.png)

当属性的类型是简单类型时，可以使用@Value注解进行注入。
```java
package com.powernode.spring6.bean4;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    @Value(value = "zhangsan")
    private String name;
    @Value("20")
    private int age;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```
开启包扫描：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.powernode.spring6.bean4"/>
</beans>
```
```java
@Test
public void testValue(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-injection.xml");
    Object user = applicationContext.getBean("user");
    System.out.println(user);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665557109935-e0300b67-fd35-4d66-99d1-dac41cb0f13d.png#averageHue=%23f3f2f0&clientId=u999e1312-db7a-4&from=paste&height=113&id=u5fa90bbd&originHeight=113&originWidth=508&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11012&status=done&style=shadow&taskId=uec092d2d-3ae1-4366-a952-c62149cdb1f&title=&width=508)

通过以上代码可以发现，我们并没有给属性提供setter方法，但仍然可以完成属性赋值。

如果提供setter方法，并且在setter方法上添加@Value注解，可以完成注入吗？尝试一下：
```java
package com.powernode.spring6.bean4;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    
    private String name;

    private int age;

    @Value("李四")
    public void setName(String name) {
        this.name = name;
    }

    @Value("30")
    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665557275282-82ba995b-6395-4d32-b322-d976ac3299d1.png#averageHue=%23f3f1ef&clientId=u999e1312-db7a-4&from=paste&height=106&id=udca1ce29&originHeight=106&originWidth=485&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11245&status=done&style=shadow&taskId=uebe9436d-a844-440b-8e6a-e14e1f6a19d&title=&width=485)

通过测试可以得知，@Value注解可以直接使用在属性上，也可以使用在setter方法上。都是可以的。都可以完成属性的赋值。
**为了简化代码，以后我们一般不提供setter方法，直接在属性上使用@Value注解完成属性赋值**。
出于好奇，我们再来测试一下，是否能够通过构造方法完成注入：
```java
package com.powernode.spring6.bean4;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {

    private String name;

    private int age;

    public User(@Value("隔壁老王") String name, @Value("33") int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665557643220-1010bea9-5578-4388-8868-4beb11dfbe95.png#averageHue=%23f2f0ee&clientId=u999e1312-db7a-4&from=paste&height=110&id=u38cd30d6&originHeight=110&originWidth=444&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11523&status=done&style=shadow&taskId=ucc4904b2-3db5-488a-896d-1b762a1d7a4&title=&width=444)

总结：
* **使用@Value的前提是：先去声明Bean，把Bean交给spring去管理**
* **使用@Value注解加在属性、加在setter和构造方法的参数上，并没有给属性提供setter方法，仍然可以完成属性赋值。**


### 2.@Autowired与@Qualifier
* **@Autowired注解可以用来注入非简单类型**。被翻译为：自动连线的，或者自动装配。
 * **单独使用@Autowired注解，默认根据类型装配**。【默认是byType】
 * **如果想要根据名字进行装配，就需要将@Autowired与@Qualifier一起使用**

看一下它的源码：
```java
package org.springframework.beans.factory.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

//可以出现在构造方法、方法、参数、属性和别的注解之上
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {
	boolean required() default true;
}
```
源码中有两处需要注意：  
- **第一处：该注解可以标注在：构造方法上、方法上、形参上、属性上和注解上**
- **第二处：该注解有一个required属性，默认值是true，表示在注入的时候要求被注入的Bean必须是存在的，如果不存在则报错。如果required属性设置为false，表示注入的Bean存在或者不存在都没关系，存在的话就注入，不存在的话，也不报错。**

我们先在属性上使用@Autowired注解：
```java
package com.powernode.spring6.dao;

public interface UserDao {
    void insert();
}
```

```java
package com.powernode.spring6.dao;

import org.springframework.stereotype.Repository;

@Repository //纳入bean管理
public class UserDaoForMySQL implements UserDao{
    @Override
    public void insert() {
        System.out.println("正在向mysql数据库插入User数据");
    }
}
```

```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service // 纳入bean管理
public class UserService {

    @Autowired // 在属性上注入
    private UserDao userDao;
    
    // 没有提供构造方法和setter方法。
    public void save(){
        userDao.insert();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.powernode.spring6.dao,com.powernode.spring6.service"/>
</beans>
```
```java
@Test
public void testAutowired(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-injection.xml");
    UserService userService = applicationContext.getBean("userService", UserService.class);
    userService.save();
}
```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665561365140-b0200308-0c25-4a29-96be-5a93594e2d2b.png#averageHue=%23f2f0ee&clientId=u999e1312-db7a-4&from=paste&height=97&id=u70f1ad5a&originHeight=97&originWidth=487&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11448&status=done&style=shadow&taskId=u76fc018c-6c0d-4446-85a2-5ce21b2b28b&title=&width=487)

以上构造方法和setter方法都没有提供，经过测试，仍然可以注入成功。

接下来，再来测试一下@Autowired注解出现在setter方法上：
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    @Autowired
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665562770986-e19377a6-af3e-4082-9463-16c795742ad5.png#averageHue=%23f1efed&clientId=u999e1312-db7a-4&from=paste&height=93&id=ue84a5069&originHeight=93&originWidth=491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11554&status=done&style=shadow&taskId=uc6cfe36b-be24-48ff-a987-885055ae1ef&title=&width=491)

我们再来看看能不能出现在构造方法上：
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    @Autowired
    public UserService(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665562985700-7820d3d8-cf43-43af-8c81-46f301ea2835.png#averageHue=%23f3f1ef&clientId=u999e1312-db7a-4&from=paste&height=111&id=u234df523&originHeight=111&originWidth=470&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11539&status=done&style=shadow&taskId=ufc7d932f-8541-4b06-83db-237a969a04a&title=&width=470)

再来看看，这个注解能不能只标注在构造方法的形参上：
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    public UserService(@Autowired UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665563225083-172d5675-cfcb-4f63-9b83-ce85b29b953e.png#averageHue=%23f4f2f0&clientId=u999e1312-db7a-4&from=paste&height=109&id=ub32f4d51&originHeight=109&originWidth=487&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11684&status=done&style=shadow&taskId=ue050ee11-60ad-40d2-a87d-7136331fa26&title=&width=487)

**还有更劲爆的，当有参数的构造方法只有一个时，@Autowired注解可以省略。但为了程序的可读性，一般不这么做**
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    public UserService(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}

```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665563320900-df9e4cb3-c046-4f5c-b482-42951f18fb16.png#averageHue=%23f4f2f0&clientId=u999e1312-db7a-4&from=paste&height=110&id=ua7390918&originHeight=110&originWidth=496&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11721&status=done&style=shadow&taskId=u28692c80-0962-4b19-af49-762cb4791d5&title=&width=496)

**当然，如果有多个构造方法，@Autowired肯定是不能省略的。**
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    public UserService(UserDao userDao) {
        this.userDao = userDao;
    }
    
    public UserService(){
        
    }

    public void save(){
        userDao.insert();
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665563410134-267b2484-54a3-4204-8e02-a9499ecbe614.png#averageHue=%23faf8f7&clientId=u999e1312-db7a-4&from=paste&height=218&id=u3fa8319b&originHeight=218&originWidth=1391&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36652&status=done&style=shadow&taskId=u8cd9a27d-178e-40b6-9a9e-6a363d63752&title=&width=1391)
到此为止，我们已经清楚@Autowired注解可以出现在哪些位置了。

@Autowired注解默认是byType进行注入的，也就是说根据类型注入的，如果以上程序中，UserDao接口还有另外一个实现类，会出现问题吗？
```java
package com.powernode.spring6.dao;

import org.springframework.stereotype.Repository;

@Repository //纳入bean管理
public class UserDaoForOracle implements UserDao{
    @Override
    public void insert() {
        System.out.println("正在向Oracle数据库插入User数据");
    }
}

```
当你写完这个新的实现类之后，此时IDEA工具已经提示错误信息了：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665563729880-0421bc02-19ca-4353-8a10-5b0ef9972b90.png#averageHue=%23faf9f8&clientId=u999e1312-db7a-4&from=paste&height=674&id=ub89c21a1&originHeight=674&originWidth=1030&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84797&status=done&style=shadow&taskId=u7f117040-8072-4719-91ad-2c7720183ee&title=&width=1030)
错误信息中说：不能装配，UserDao这个Bean的数量大于1.

怎么解决这个问题呢？**当然要byName，根据名称进行装配了。@Autowired注解和@Qualifier注解联合起来才可以根据名称进行装配，在@Qualifier注解中指定自动装配的Bean名称。**
```java
package com.powernode.spring6.dao;

import org.springframework.stereotype.Repository;

@Repository // 这里没有给bean起名，默认名字是：userDaoForOracle
public class UserDaoForOracle implements UserDao{
    @Override
    public void insert() {
        System.out.println("正在向Oracle数据库插入User数据");
    }
}

```
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    @Autowired
    @Qualifier("userDaoForOracle") // 这个是bean的名字。
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}
```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665564055076-ffda3ad0-f957-4216-bf6c-957d62724d5f.png#averageHue=%23f2f0ed&clientId=u999e1312-db7a-4&from=paste&height=97&id=u149f27ad&originHeight=97&originWidth=484&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11655&status=done&style=shadow&taskId=u1d416395-88a7-4458-be3a-e0c8cba234e&title=&width=484)
总结：  
* **@Autowired可以标注在：构造方法上、方法上、形参上、属性上和注解上**
* - 当带参数的构造方法只有一个，@Autowired注解可以省略。
* **使用@Autowired时，如果是写在属性上，那么被注入的Bean和Bean本身都要声明，不需要指定任何属性，直接使用即可。并且构造方法和setter方法都没有提供，经过测试，仍然可以注入成功**
* **单独使用@Autowired注解，默认根据类型装配。【默认是byType】。如果想要根据名字进行装配，就需要将@Autowired与@Qualifier一起使用**
- **如果dao层一个接口他的实现类有多个，并且在service层中的属性的编译类型是接口类型，那么此时必须要根据名称进行装配。因为框架不知道需要将哪一个类型的实例装配给该属性**
- **@Autowired注解和@Qualifier注解联合起来才可以根据名称进行装配，@Autowired写在@Qualifier上方，在@Qualifier注解中指定自动装配的Bean名称，默认情况下是类名首字母小写**

==**如果把value属性彻底去掉，spring会被Bean自动取名吗？会的。并且默认名字的规律是：Bean类名首字母小写即可。**==
### 3. @Resource
**@Resource注解也可以完成非简单类型注入**。那它和@Autowired注解有什么区别？

- **@Resource注解是JDK扩展包中的，也就是说属于JDK的一部分。所以该注解是标准注解**，更加具有通用性。(**@Resource注解是官方建议使用该注解，并且要比@Autowired使用频率高一些**)
- **@Autowired注解是Spring框架自己的**。
- ==**@Resource注解默认根据名称装配byName，通过name属性来指定Bean的名称。未指定name时，使用属性名作为name。通过name找不到的话会自动启动通过类型byType装配。**==
- **@Autowired注解默认根据类型装配byType，如果想根据名称装配，需要配合@Qualifier注解一起用。**
- **@Resource注解用在属性上、setter方法上，不能出现在构造方法上**。
- @Autowired注解用在属性上、setter方法上、构造方法上、构造方法参数上。

@Resource注解属于JDK扩展包，所以不在JDK当中，需要额外引入以下依赖：【**如果是JDK8的话不需要额外引入依赖。高于JDK11或低于JDK8需要引入以下依赖。**】
```xml
<dependency>
  <groupId>jakarta.annotation</groupId>
  <artifactId>jakarta.annotation-api</artifactId>
  <version>2.1.1</version>
</dependency>
```
一定要注意：**如果你用Spring6，要知道Spring6不再支持JavaEE，它支持的是JakartaEE9。（Oracle把JavaEE贡献给Apache了，Apache把JavaEE的名字改成JakartaEE了，大家之前所接触的所有的  javax.\*  包名统一修改为  jakarta.*包名了。）**
```xml
<dependency>
  <groupId>javax.annotation</groupId>
  <artifactId>javax.annotation-api</artifactId>
  <version>1.3.2</version>
</dependency>
```

@Resource注解的源码如下：   
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665565515435-2ad5614a-8572-4c6f-80c1-efa236dbe35f.png#averageHue=%23fcf8f6&clientId=u999e1312-db7a-4&from=paste&height=426&id=u6038d063&originHeight=426&originWidth=1066&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71322&status=done&style=shadow&taskId=u62cc11b0-bd1d-4e76-8123-cbf4e5556cb&title=&width=1066)
测试一下：
```java
package com.powernode.spring6.dao;

import org.springframework.stereotype.Repository;

@Repository("xyz")
public class UserDaoForOracle implements UserDao{
    @Override
    public void insert() {
        System.out.println("正在向Oracle数据库插入User数据");
    }
}
```

```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Resource(name = "xyz")
    private UserDao userDao;

    public void save(){
        userDao.insert();
    }
}
```
执行测试程序：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665622877352-0ae69e3c-e7f3-452d-a405-392901612465.png#averageHue=%23f3f1ef&clientId=u27fdab07-dcc6-4&from=paste&height=112&id=u10e932c1&originHeight=112&originWidth=467&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11805&status=done&style=shadow&taskId=u6c533807-043a-4995-862f-5f10be7c2e4&title=&width=467)

**我们把UserDaoForOracle的名字xyz修改为userDao，让这个Bean的名字和UserService类中的UserDao属性名一致：**
```java
package com.powernode.spring6.dao;

import org.springframework.stereotype.Repository;

@Repository("userDao")
public class UserDaoForOracle implements UserDao{
    @Override
    public void insert() {
        System.out.println("正在向Oracle数据库插入User数据");
    }
}
```

```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Resource
    private UserDao userDao;

    public void save(){
        userDao.insert();
    }
}

```
执行测试程序：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665623044796-c4051a04-c56b-4ce9-b627-333ab7ca7b6a.png#averageHue=%23f3f1ef&clientId=u27fdab07-dcc6-4&from=paste&height=103&id=u0824fe26&originHeight=103&originWidth=479&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11673&status=done&style=shadow&taskId=u030d8a0e-b7df-40f1-bf91-5f93056777a&title=&width=479)
通过测试得知，**当@Resource注解使用时没有指定name的时候，还是根据name进行查找，这个name是属性名。**

接下来把UserService类中的属性名修改一下：
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Resource
    private UserDao userDao2;

    public void save(){
        userDao2.insert();
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665623273523-aff8ef45-b484-4462-bacc-fba7e14c8fee.png#averageHue=%23fbf8f7&clientId=u27fdab07-dcc6-4&from=paste&height=248&id=u0c222c16&originHeight=248&originWidth=1585&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23269&status=done&style=shadow&taskId=u40b6ff92-b1cc-4056-a77c-9b171aaab93&title=&width=1585)
* 根据异常信息得知：显然当通过name找不到的时候，自然会启动byType进行注入。以上的错误是因为UserDao接口下有两个实现类导致的。所以根据类型注入就会报错。

我们再来看@Resource注解使用在setter方法上可以吗？
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    @Resource
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}

```
注意这个setter方法的方法名，setUserDao去掉set之后，将首字母变小写userDao，userDao就是name

执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665623530366-79b8e09d-2559-4657-83eb-0b722261045f.png#averageHue=%23f3f1ef&clientId=u27fdab07-dcc6-4&from=paste&height=105&id=uce4c028f&originHeight=105&originWidth=482&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11743&status=done&style=shadow&taskId=u9b8d145f-59f7-4816-9e5f-1cdd64c8d24&title=&width=482)

当然，也可以指定name：
```java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private UserDao userDao;

    @Resource(name = "userDaoForMySQL")
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665623611980-a66591e7-bd29-4327-a43c-6c6492c8612f.png#averageHue=%23f2f0ee&clientId=u27fdab07-dcc6-4&from=paste&height=96&id=ue56a8b89&originHeight=96&originWidth=489&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11431&status=done&style=shadow&taskId=u535fea63-6aa6-4a33-bc17-453192a7468&title=&width=489)

一句话总结@Resource注解：默认byName注入，没有指定name时把属性名当做name，根据name找不到时，才会byType注入。byType注入时，某种类型的Bean只能有一个。

