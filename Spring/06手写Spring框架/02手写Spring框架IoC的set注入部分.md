* **Spring IoC容器的实现原理：工厂模式 + 解析XML + 反射机制。**
* **Java语言解析xml文件使用的组件是dom4j和jaxen，所以需要引入jar包**

## 一、准备工作
#### 1.创建项目引入依赖
采用Maven方式新建Module：myspring  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665475207334-bd779f04-b490-4237-9ab1-306989458f22.png#averageHue=%23f3f1f1&clientId=ue12e8566-378b-4&from=paste&height=602&id=u9efa03bf&originHeight=602&originWidth=772&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40674&status=done&style=shadow&taskId=u4db572af-dfb7-4fe3-8647-149614b6e24&title=&width=772)

打包方式采用jar，并且引入dom4j和jaxen的依赖，因为要使用它解析XML文件，还有junit依赖。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.myspringframework</groupId>
    <artifactId>myspring</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>2.1.3</version>
        </dependency>
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.2.0</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```


#### 2.准备好我们要管理的Bean
准备好我们要管理的Bean（**这些Bean在将来开发完框架之后是要删除的**）

```java
package com.powernode.myspring.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Address
 * @since 1.0
 **/
public class Address {
    private String city;
    private String street;
    private String zipcode;

    public Address() {
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getStreet() {
        return street;
    }

    public void setStreet(String street) {
        this.street = street;
    }

    public String getZipcode() {
        return zipcode;
    }

    public void setZipcode(String zipcode) {
        this.zipcode = zipcode;
    }

    @Override
    public String toString() {
        return "Address{" +
                "city='" + city + '\'' +
                ", street='" + street + '\'' +
                ", zipcode='" + zipcode + '\'' +
                '}';
    }
}

```

```java
package com.powernode.myspring.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className User
 * @since 1.0
 **/
public class User {
    private String name;
    private int age;
    private Address addr;

    public User() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Address getAddr() {
        return addr;
    }

    public void setAddr(Address addr) {
        this.addr = addr;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", addr=" + addr +
                '}';
    }
}

```


#### 3.准备myspring.xml配置文件
将来在框架开发完毕之后，这个文件也是要删除的。因为这个配置文件的提供者应该是使用这个框架的程序员。

文件名随意，我们这里叫做：myspring.xml
文件放在类路径当中即可，我们这里把文件放到类的根路径下。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>

    <bean id="userBean" class="com.powernode.myspring.bean.User">
        <property name="name" value="张三"/>
        <property name="age" value="20"/>
        <property name="addr" ref="addrBean"/>
    </bean>
    
    <bean id="addrBean" class="com.powernode.myspring.bean.Address">
        <property name="city" value="北京"/>
        <property name="street" value="大兴区"/>
        <property name="zipcode" value="1000001"/>
    </bean>

</beans>
```
使用value给简单属性赋值。使用ref给非简单属性赋值。


## 二、确定核心接口和实现类

1. **ApplicationContext接口中提供一个getBean()方法，通过该方法可以获取Bean对象。**

注意包名：这个接口就是myspring框架中的一员了。
```java
package org.myspringframework.core;

/**
 * @author 动力节点
 * @version 1.0
 * @className ApplicationContext
 * @since 1.0
 **/
public interface ApplicationContext {
    /**
     * 根据bean的id获取bean实例。
     * @param beanId bean的id
     * @return bean实例
     */
    Object getBean(String beanId);
}

```

2. **编写接口实现类ClassPathXmlApplicationContext**

**ClassPathXmlApplicationContext是ApplicationContext接口的实现类。该类从类路径当中加载myspring.xml配置文件。**
```java
package org.myspringframework.core;

/**
 * @author 动力节点
 * @version 1.0
 * @className ClassPathXmlApplicationContext
 * @since 1.0
 **/
public class ClassPathXmlApplicationContext implements ApplicationContext{
    @Override
    public Object getBean(String beanId) {
        return null;
    }
}

```


## 三、编写实现类内部具体实现
**`ApplicationContext applicationContext = new ClassPathXmlApplicationContext("myspring.xml");`该句话只要一执行，底层会解析spring配置文件，即xml解析。之后把所有的bean对创建出来，交给一个map去进行存储。**

1. **确定采用Map集合存储Bean实例。Map集合的key存储beanId，value存储Bean实例。Map<String,Object>。同时实现getBean方法。**
```java
package org.myspringframework.core;

import java.util.HashMap;
import java.util.Map;

/**
 * @author 动力节点
 * @version 1.0
 * @className ClassPathXmlApplicationContext
 * @since 1.0
 **/
public class ClassPathXmlApplicationContext implements ApplicationContext{
    /**
     * 存储bean的Map集合
     */
    private Map<String,Object> beanMap = new HashMap<>();

    /**
     * 在该构造方法中，解析myspring.xml文件，创建所有的Bean实例，并将Bean实例存放到Map集合中。
     * @param resource 配置文件路径（要求在类路径当中）
     */
    public ClassPathXmlApplicationContext(String resource) {

    }

    @Override
    public Object getBean(String beanId) {
        return beanMap.get(beanId);
    }
}

```

2. **在ClassPathXmlApplicationContext的构造方法中解析配置文件，获取所有bean的类名，通过反射机制调用无参数构造方法创建Bean。并且将Bean对象存放到Map缓存集合中。**
```java
/**
* 在该构造方法中，解析myspring.xml文件，创建所有的Bean实例，并将Bean实例存放到Map集合中。
* @param resource 配置文件路径（要求在类路径当中）
*/
public ClassPathXmlApplicationContext(String resource) {
    try {
	    // 解析myspring.xml文件，然后实例化Bean，将Bean存放到singletonObjects集合当中。  
		// 这是dom4j解析XML文件的核心对象。
        SAXReader reader = new SAXReader();
        // 获取一个输入流，指向配置文件。通过类加载器可以获取系统类加载器，之后调用getResourceAsStream()代表获取指定类路径的输入流
        InputStream in = ClassLoader.getSystemClassLoader().getResourceAsStream(configLocation);  
		// 读文件，Document对象就代表读到的配置文件  
		Document document = reader.read(in);
        // 获取所有的bean标签，Node就是封装一个Bean标签以及其中内容的类
        List<Node> beanNodes = document.selectNodes("//bean");
        // 遍历集合
        beanNodes.forEach(beanNode -> {
		    // 向下转型的目的是为了使用Element接口里更加丰富的方法。Element接口实现了Node接口
            Element beanElt = (Element) beanNode;
            // 获取id
            String id = beanElt.attributeValue("id");
            // 获取全限定类名
            String className = beanElt.attributeValue("class");
            try {
                // 通过反射机制创建对象
                Class<?> clazz = Class.forName(className);
                // 获取无参数构造方法
                Constructor<?> defaultConstructor = clazz.getDeclaredConstructor();
                // 调用无参数构造方法实例化Bean
                Object bean = defaultConstructor.newInstance();
                //在赋值前先加入Map集合，将Bean曝光
                beanMap.put(id, bean);
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

3. 测试能否获取到Bean

编写测试程序。
```java
package com.powernode.myspring.test;

import org.junit.Test;
import org.myspringframework.core.ApplicationContext;
import org.myspringframework.core.ClassPathXmlApplicationContext;

/**
 * @author 动力节点
 * @version 1.0
 * @className MySpringTest
 * @since 1.0
 **/
public class MySpringTest {
    @Test
    public void testMySpring(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("myspring.xml");
        Object userBean = applicationContext.getBean("userBean");
        Object addrBean = applicationContext.getBean("addrBean");
        System.out.println(userBean);
        System.out.println(addrBean);
    }
}

```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665478707450-7e52f70c-97b2-4e6d-b96f-cc5bea8b51a4.png#averageHue=%23f8f7f6&clientId=ud56a2a07-f6fd-4&from=paste&height=150&id=u2c1d3f33&originHeight=150&originWidth=649&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17012&status=done&style=shadow&taskId=uf5600011-32e6-4ec6-8824-7431503ca2f&title=&width=649)

通过测试Bean已经实例化成功了，属性的值是null，这是我们能够想到的，毕竟我们调用的是无参数构造方法，所以属性都是默认值。


4. **通过反射机制调用set方法，给Bean的属性赋值。**

继续在ClassPathXmlApplicationContext构造方法中编写代码。
```java
package org.myspringframework.core;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;

import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author 动力节点
 * @version 1.0
 * @className ClassPathXmlApplicationContext
 * @since 1.0
 **/
public class ClassPathXmlApplicationContext implements ApplicationContext{
    /**
     * 存储bean的Map集合
     */
    private Map<String,Object> beanMap = new HashMap<>();

    /**
     * 在该构造方法中，解析myspring.xml文件，创建所有的Bean实例，并将Bean实例存放到Map集合中。
     * @param resource 配置文件路径（要求在类路径当中）
     */
    public ClassPathXmlApplicationContext(String resource) {
        try {
	    // 解析myspring.xml文件，然后实例化Bean，将Bean存放到singletonObjects集合当中。  
		// 这是dom4j解析XML文件的核心对象。
        SAXReader reader = new SAXReader();
        // 获取一个输入流，指向配置文件。通过类加载器可以获取系统类加载器，之后调用getResourceAsStream()代表获取指定路径的输入流
        InputStream in = ClassLoader.getSystemClassLoader().getResourceAsStream(configLocation);  
		// 读文件，Document对象就代表读到的配置文件  
		Document document = reader.read(in);
        // 获取所有的bean标签，Node就是封装一个Bean标签以及其中内容的类
        List<Node> beanNodes = document.selectNodes("//bean");
        // 遍历集合
        beanNodes.forEach(beanNode -> {
		    // 向下转型的目的是为了使用Element接口里更加丰富的方法。Element接口实现了Node接口
            Element beanElt = (Element) beanNode;
            // 获取id
            String id = beanElt.attributeValue("id");
            // 获取全限定类名
            String className = beanElt.attributeValue("class");
            try {
                // 通过反射机制创建对象
                Class<?> clazz = Class.forName(className);
                // 获取无参数构造方法
                Constructor<?> defaultConstructor = clazz.getDeclaredConstructor();
                // 调用无参数构造方法实例化Bean
                Object bean = defaultConstructor.newInstance();
                //在赋值前先加入Map集合，将Bean曝光
                beanMap.put(id, bean);
            } catch (Exception e) {
                e.printStackTrace();
            }
            // 再重新遍历集合，这次遍历是为了给Bean的所有属性赋值。
            // 思考：为什么不在上面的循环中给Bean的属性赋值，而在这里再重新遍历一次呢？
            // 通过这里你是否能够想到Spring是如何解决循环依赖的：实例化和属性赋值分开。
            beanNodes.forEach(beanNode -> {
                Element beanElt = (Element) beanNode;
                // 获取bean的id
                String beanId = beanElt.attributeValue("id");
                // 取该bean标签下所有的属性property标签
                List<Element> propertyElts = beanElt.elements("property");
                // 遍历所有属性
                propertyElts.forEach(propertyElt -> {
                    try {
                        // 获取属性名
                        String propertyName = propertyElt.attributeValue("name");
                        // 获取属性类型
                        Class<?> propertyType = beanMap.get(beanId).getClass().getDeclaredField(propertyName).getType();
                        // 获取set方法名
                        String setMethodName = "set" + propertyName.toUpperCase().charAt(0) + propertyName.substring(1);
                        // 获取set方法
                        Method setMethod = beanMap.get(beanId).getClass().getDeclaredMethod(setMethodName, propertyType);
                        // 调用set方法(set方法没有返回值)
                        // 获取属性的值，值可能是value，也可能是ref。
                        // 获取value
                        String propertyValue = propertyElt.attributeValue("value");// "30"
                        // 获取ref
                        String propertyRef = propertyElt.attributeValue("ref");
                        Object propertyVal = null; //这是Object类的真值，暂时未确定什么类型
                        if (propertyValue != null) {
                            // 该属性是简单属性
                            // 我们myspring框架声明一下：我们只支持这些类型为简单类型  
							// byte short int long float double boolean char  
							// Byte Short Integer Long Float Double Boolean Character  
							// String
							// 获取属性类型名，简单名：去掉包名
                            String propertyTypeSimpleName = propertyType.getSimpleName();
                            switch (propertyTypeSimpleName) {
                                case "byte": case "Byte":
                                    propertyVal = Byte.valueOf(propertyValue);
                                    break;
                                case "short": case "Short":
                                    propertyVal = Short.valueOf(propertyValue);
                                    break;
                                case "int": case "Integer":
                                    propertyVal = Integer.valueOf(propertyValue);
                                    break;
                                case "long": case "Long":
                                    propertyVal = Long.valueOf(propertyValue);
                                    break;
                                case "float": case "Float":
                                    propertyVal = Float.valueOf(propertyValue);
                                    break;
                                case "double": case "Double":
                                    propertyVal = Double.valueOf(propertyValue);
                                    break;
                                case "boolean": case "Boolean":
                                    propertyVal = Boolean.valueOf(propertyValue);
                                    break;
                                case "char": case "Character":
                                    propertyVal = propertyValue.charAt(0);
                                    break;
                                case "String":
                                    propertyVal = propertyValue;
                                    break;
                            }
                            //调用set方法(set方法没有返回值)
                            setMethod.invoke(beanMap.get(beanId), propertyVal);
                        }
                        if (propertyRef != null) {
                            // 该属性不是简单属性
                            // 调用set方法(set方法没有返回值)
                            setMethod.invoke(beanMap.get(beanId), beanMap.get(propertyRef));
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                });
            });

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public Object getBean(String beanId) {
        return beanMap.get(beanId);
    }
}

```
* 重点处理：当property标签中是value怎么办？是ref怎么办？

执行测试程序：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665481050714-a41f73d9-67bb-40b9-9137-601a0775450d.png#averageHue=%23f9f7f6&clientId=ud56a2a07-f6fd-4&from=paste&height=147&id=udfde2846&originHeight=147&originWidth=969&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25735&status=done&style=shadow&taskId=u4a07ea37-3e47-4a10-a42a-19f2547abf6&title=&width=969)


## 四、打包发布及使用

##### 1.打包发布
将多余的类以及配置文件删除，使用maven打包发布。  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665481384984-b9b107a7-6566-473a-95df-fc7fcb613f18.png#averageHue=%23f9f8f7&clientId=ud56a2a07-f6fd-4&from=paste&height=222&id=uc26e4de5&originHeight=222&originWidth=395&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7445&status=done&style=shadow&taskId=ua39a7c0a-fa64-442a-bb7f-3e6dd9c2bf3&title=&width=395)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665481462831-bbd5bfd3-d647-4c04-990a-9c39a4116d21.png#averageHue=%23f9f9f8&clientId=ud56a2a07-f6fd-4&from=paste&height=173&id=u5e804933&originHeight=173&originWidth=533&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13326&status=done&style=shadow&taskId=u8a00a74f-aecc-4600-a3d8-8c359026ca3&title=&width=533)
##### 2.站在程序员角度使用myspring框架
新建模块：myspring-test    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665481605553-46ba6264-a360-4700-a696-1aa536c44cf1.png#averageHue=%23f3f2f2&clientId=ud56a2a07-f6fd-4&from=paste&height=604&id=u11f4822d&originHeight=604&originWidth=778&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39797&status=done&style=shadow&taskId=uf7fae99d-0f9f-412b-b003-d05456f6acc&title=&width=778)
引入myspring框架的依赖：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.powernode</groupId>
    <artifactId>myspring-test</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.myspringframework</groupId>
            <artifactId>myspring</artifactId>
            <version>1.0.0</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```
编写Bean
```java
package com.powernode.myspring.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className UserDao
 * @since 1.0
 **/
public class UserDao {
    public void insert(){
        System.out.println("UserDao正在插入数据");
    }
}

```
```java
package com.powernode.myspring.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className UserService
 * @since 1.0
 **/
public class UserService {
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        System.out.println("UserService开始执行save操作");
        userDao.insert();
        System.out.println("UserService执行save操作结束");
    }
}

```
编写myspring.xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans>

    <bean id="userServiceBean" class="com.powernode.myspring.bean.UserService">
        <property name="userDao" ref="userDaoBean"/>
    </bean>

    <bean id="userDaoBean" class="com.powernode.myspring.bean.UserDao"/>

</beans>
```
编写测试程序
```java
package com.powernode.myspring.test;

import com.powernode.myspring.bean.UserService;
import org.junit.Test;
import org.myspringframework.core.ApplicationContext;
import org.myspringframework.core.ClassPathXmlApplicationContext;

/**
 * @author 动力节点
 * @version 1.0
 * @className MySpringTest
 * @since 1.0
 **/
public class MySpringTest {

    @Test
    public void testMySpring(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("myspring.xml");
        UserService userServiceBean = (UserService) applicationContext.getBean("userServiceBean");
        userServiceBean.save();
    }
}

```
执行结果  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665482096446-2015f7a8-3e86-4d74-a26b-a3d417c250fa.png#averageHue=%23f8f7f5&clientId=ud56a2a07-f6fd-4&from=paste&height=167&id=u1daaa772&originHeight=167&originWidth=425&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19370&status=done&style=shadow&taskId=u97aace1a-88b7-4a63-86f2-aa0d860ea1b&title=&width=425)

