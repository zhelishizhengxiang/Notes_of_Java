
### 1.绑定简单bean
* **SpringBoot配置文件中的信息除了可以使用`@Value注解`读取之外，也可以将配置信息一次性赋值给Bean对象的属性，并且比@Value注解好用很多。**

例如有这样的配置：

`application.yml`

```yaml
app:
  name: jack
  age: 30
  email: jack@123.com
```

Bean需要这样定义：

```java
package com.powernode.sb307externalconfig.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "app")
public class AppBean {
    private String name;
    private Integer age;
    private String email;

    @Override
    public String toString() {
        return "AppBean{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", email='" + email + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

说明：
1. **被绑定的bean，需要使用`@ConfigurationProperties(prefix = "app")`注解进行标注，prefix用来指定前缀，哪个是前缀，如下图所示**：  
	![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729667789270-8c85788b-8b00-4d17-bbab-5f87b9a68103.png)
2. **配置文件中的`name`、`age`、`email`要和bean对象的属性名`name`、`age`、`email`对应上。（属性名相同）并且bean中的所有属性都提供了`setter`方法。因为底层是通过`setter`方法给bean属性赋值的。**
3. .**这样的bean需要使用`@Component`注解进行标注，纳入IoC容器的管理（@Configuration注解也可以）。`@Component`注解负责创建Bean对象，`@ConfigurationProperties(prefix = "app")`注解负责给bean对象的属性赋值。**
4. **bean的属性需要是`非static`的属性。**


编写测试程序，将bean对象输出，结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729668174305-f203b7f0-9ed2-435a-8c89-0f385cc6f50b.png)




### 2.@Configuration注解
* **以上操作中使用了`@Component注解`进行了标注，来纳入IoC容器的管理。也可以使用另外一个注解`@Configuration`，用这个注解将Bean标注为配置类。该注解标注的类也会被纳入IOC管理**
* **多数情况下我们会选择使用这个注解，因为该Bean对象的属性对应的就是配置文件中的配置信息，因此这个Bean我们也可以将其看做是一个配置类。**

```java
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppBean {
    private String name;
    private Integer age;
    private String email;
    //setter and getter
}
```

运行测试程序：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729671038234-ecf76053-a6d2-4100-8942-cf246e71397c.png)




我们把这个Bean对象的类名打印一下看看：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729671124931-0ce60aa3-54bb-4a35-9b99-c846f86ad6b8.png)

可以发现底层实际上创建了`AppBean`的代理对象`AppBean$$SpringCGLIB`。

生成代理对象会影响效率，这里我们不需要使用代理功能，可以通过以下配置来取消代理机制：

```java
@Configuration(proxyBeanMethods = false)
@ConfigurationProperties(prefix = "app")
public class AppBean {
    private String name;
    private Integer age;
    private String email;
    //setter and getter
}
```

执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729671341895-443510ce-3206-4183-823c-a65fbd402b91.png)





### 3.绑定嵌套bean
**当一个Bean中嵌套了一个Bean，这种情况下可以将配置信息绑定到该Bean上吗？当然可以。**

有这样的一个配置：

```yaml
app:
  name: jack
  age: 30
  email: jack@123.com
  address: 
    city: BJ
    street: ChaoYang
    zipcode: 123456
```

需要编写这样的两个Bean：

```java
package com.powernode.sb307externalconfig.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
@ConfigurationProperties(prefix = "app")
public class AppBean {
    private String name;
    private Integer age;
    private String email;
    private Address address;

    @Override
    public String toString() {
        return "AppBean{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", email='" + email + '\'' +
                ", address=" + address +
                '}';
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}

```



```java
package com.powernode.sb307externalconfig.bean;

public class Address {
    private String city;
    private String street;
    private String zipcode;

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

执行测试程序，结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729671962724-dd353f23-2e0b-44c3-9965-c9d996960fef.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1730804471502-31c64115-c49a-4d76-92e0-90e9ae543b32.png)

### 4.@EnableConfigurationProperties与@ConfigurationPropertiesScan
**将`AppBean`纳入IoC容器的管理，之前我们说了两种方式：第一种是使用`@Component`，第二种是使用`@Configuration`。SpringBoot其实还提供了另外两种方式，这两个注解都是标注在SpringBoot主入口程序上的：**
+ **第一种：@EnableConfigurationPropertie（用的较多）**
+ **第二种：@ConfigurationPropertiesScan**


```java
@EnableConfigurationProperties(AppBean.class)
@SpringBootApplication
public class Sb307ExternalConfigApplication {
    public static void main(String[] args) {
        SpringApplication.run(Sb307ExternalConfigApplication.class, args);
    }
}
```

或者

```java
@ConfigurationPropertiesScan(basePackages = "com.powernode.sb307externalconfig.bean")
@SpringBootApplication
public class Sb307ExternalConfigApplication {
    public static void main(String[] args) {
        SpringApplication.run(Sb307ExternalConfigApplication.class, args);
    }
}
```

运行测试程序，执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729673145917-468caf80-bf36-4931-91f5-a865ec0dfb14.png)





### 5.将配置赋值到Bean的Map/List/Array属性上
代码如下：

```java
package com.powernode.sb307externalconfig.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;

import java.util.Arrays;
import java.util.List;
import java.util.Map;

@ConfigurationProperties
public class CollectionConfig {
    private String[] names;
    private List<Product> products;
    private Map<String, Vip> vips;

    @Override
    public String toString() {
        return "CollectionConfig{" +
                "names=" + Arrays.toString(names) +
                ", products=" + products +
                ", vips=" + vips +
                '}';
    }

    public String[] getNames() {
        return names;
    }

    public void setNames(String[] names) {
        this.names = names;
    }

    public List<Product> getProducts() {
        return products;
    }

    public void setProducts(List<Product> products) {
        this.products = products;
    }

    public Map<String, Vip> getVips() {
        return vips;
    }

    public void setVips(Map<String, Vip> vips) {
        this.vips = vips;
    }
}

class Product {
    private String name;
    private Double price;

    @Override
    public String toString() {
        return "Product{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }
}

class Vip {
    private String name;
    private Integer age;

    @Override
    public String toString() {
        return "Vip{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

配置信息如下：application.properties
```properties
app2.abc.names[0]=jack  
app2.abc.names[1]=lucy  
#数组的配置
app2.abc.addrArray[0].city=BeiJing  
app2.abc.addrArray[0].street=ChaoYang  
app2.abc.addrArray[1].city=TianJin  
app2.abc.addrArray[1].street=NanKai  

  #list的配置
app2.abc.addrList[0].city=BeiJing_List  
app2.abc.addrList[0].street=ChaoYang_List  
app2.abc.addrList[1].city=TianJin_List  
app2.abc.addrList[1].street=NanKai_List  

# 这是map个配置，key是addr1和addr2
app2.abc.addrs.addr1.city=BeiJing_Map  
app2.abc.addrs.addr1.street=ChaoYang_Map  
app2.abc.addrs.addr2.city=TianJin_Map  
app2.abc.addrs.addr2.street=NanKai_Map
```

配置信息如下：`application.yml`
```yaml
#数组
names:
  - jackson
  - lucy
  - lili
# 或者
names: [jack, lucy]

#List集合
products: 
  - name: 西瓜
    price: 3.0
  - name: 苹果
    price: 2.0

#Map集合
vips:
  #这是key，在缩进一次就是value
  vip1:
    name: 张三
    age: 20
  vip2:
    name: 李四
    age: 22

#注意在yml和properties中遇到驼峰命名可以改成下面的配置，这样用的最多
#    addrArray:  
    addr-array:
```

提醒：记得入口程序使用@ConfigurationPropertiesScan(basePackages = "com.powernode.sb307externalconfig.bean")进行标注。

<font style="color:#080808;background-color:#ffffff;"></font>

<font style="color:#080808;background-color:#ffffff;">编写测试程序，执行结果如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729680626317-8b45a7b3-9117-47da-ac28-f153ac4d3d49.png)





### 6.将配置绑定到第三方对象
* **将配置文件中的信息绑定到某个Bean对象上，如果这个Bean对象没有源码**，是第三方库提供的，怎么办？
* **此时可以单独编写一个方法，在方法上使用以下两个注解进行标注**：
	+ **@Bean：用于在配置类中声明这是一个Bean，将来要纳入ioc容器管理**
	+ **@ConfigurationProperties：用于去注入属性，值为配置文件中的值**

假设我们有这样一个类`Address`，代码如下：

```java
package com.powernode.sb307externalconfig.bean;

public class Address {
    private String city;
    private String street;
    private String zipcode;

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

当然，我们是看不到这个源码的，只知道有这样一个字节码`Address.class`。大家也可以看到这个`Address`类上没有添加任何注解。假设我们要将以下配置绑定到这个Bean上应该怎么做？

```yaml
address:
  city: TJ
  street: XiangYangLu
  zipcode: 11111111
```



实现代码如下：

```java
@Configuration
public class ApplicationConfig {
    @Bean
    @ConfigurationProperties(prefix = "address")
    public Address getAddress(){
        return new Address();
    }
}
```

运行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729674936611-e86b3cad-c910-4f00-bb9f-8d5f976f8d94.png)



### 7.指定数据来源
之前所讲的内容是将Spring Boot框架默认的配置文件`application.properties`或`application.yml`作为数据的来源绑定到Bean上。**如果配置信息没有在默认的配置文件中呢？可以使用@PropertySource注解指定配置文件的位置，这个配置文件可以是`.properties`，也可以是`.xml`。这里重点掌握`.properties`即可。**

在`resources`目录下新建`a`目录，在`a`目录下新建`b`目录，`b`目录中新建`group-info.properties`文件，进行如下的配置：

```properties
group.name=IT
group.leader=LaoDu
group.count=20
```

定义Java类`Group`，然后进行注解标注：

```java
package com.powernode.sb307externalconfig.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@Configuration
@ConfigurationProperties(prefix = "group")
@PropertySource("classpath:a/b/group-info.properties")
public class Group {
    private String name;
    private String leader;
    private Integer count;

    @Override
    public String toString() {
        return "Group{" +
                "name='" + name + '\'' +
                ", leader='" + leader + '\'' +
                ", count=" + count +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getLeader() {
        return leader;
    }

    public void setLeader(String leader) {
        this.leader = leader;
    }

    public Integer getCount() {
        return count;
    }

    public void setCount(Integer count) {
        this.count = count;
    }
}

```

以下三个注解分别起到什么作用：

+ **@Configuration：指定该类为配置类，纳入Spring容器的管理**
+ **@ConfigurationProperties(prefix = "group")：将配置文件中的值赋值给Bean对象的属性**
+ **@PropertySource("classpath:a/b/group-info.properties")：指定额外的配置文件**



编写测试程序，测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729681829431-7e25af75-4618-410a-a5c7-d377c53683d9.png)


### 8.@ImportResource注解（了解）
创建Bean的三种方式总结：
+ 第一种方式：编写applicationContext.xml文件，在该文件中注册Bean，Spring容器启动时实例化配置文件中的Bean对象。
+ **第二种方式：@Configuration注解结合@Bean注解**。
+ **第三种方式：@Component、@Service、@Controller、@Repository等注解**。

第二种和第三种我们都已经知道了。针对第一种方式，如果在SpringBoot框架中应该怎么实现呢？使用@ImportResource注解实现


定义一个普通的Java类：Person

```java
package com.powernode.sb307externalconfig.bean;

public class Person {
    private String name;
    private String age;

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age='" + age + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }
}

```



在`resources`目录下新建`applicationContext.xml`配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="person" class="com.powernode.sb307externalconfig.bean.Person">
        <property name="name" value="jackson"/>
        <property name="age" value="20"/>
    </bean>
</beans>
```



在SpringBoot主入口类上添加@ImportResource进行资源导入，这样`applicationContext.xml`文件中的Bean将会纳入IoC容器的管理：

```java
@ImportResource("classpath:applicationContext.xml")
public class Sb307ExternalConfigApplication {}
```



编写测试程序，看看是否可以获取到`person`这个bean对象：

```java
@SpringBootTest
class Sb307ExternalConfigApplicationTests {
    @Autowired
    private Person person;
    @Test
    void test09(){
        System.out.println(person);
    }
}
```

执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729683600179-80efe94a-9aca-4959-a6ee-af938bb4fc61.png)


因此，**项目中如果有类似于Spring的这种xml配置文件，要想纳入IoC容器管理，需要在入口类上使用`@ImportResource("classpath:applicationContext.xml")`注解即可**。







## 9.Environment
* **SpringBoot框架在启动的时候会将系统配置，环境信息全部封装到对象中，如果要获取这些环境信息，可以调用Environment接口（Envirobnment对象）的方法。**
* 在Spring Boot中，`Environment`接口提供了访问应用程序环境信息的方法，比如活动配置文件、系统环境变量、命令行参数等。`Environment`接口由Spring框架提供，Spring Boot应用程序通常会使用Spring提供的实现类`AbstractEnvironment`及其子类来实现具体的环境功能。
* `Environment`对象封装的主要数据包括：
	1. **Active Profiles: 当前激活的配置文件列表**。Spring Boot允许应用程序定义不同的环境配置文件（如开发环境、测试环境和生产环境），通过激活不同的配置文件来改变应用程序的行为。
	2. **System Properties: 系统属性**，通常是操作系统级别的属性，比如操作系统名称、Java版本等。
	3. **System Environment Variables: 系统环境变量**，这些变量通常是由操作系统提供的，可以在启动应用程序时设置特定的值。
	4. **Command Line Arguments：应用程序启动时传递给主方法的命令行参数**。
	5. **Property Sources: `Environment`还包含了一个`PropertySource`列表，这个列表包含了从不同来源加载的所有属性**。`PropertySource`可以来自多种地方，比如配置文件、系统属性、环境变量等。

在Spring Boot中，可以通过注入`Environment`来获取上述信息。例如：
```java
package com.powernode.springboot.bean;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Component;

@Component
public class SomeBean {

    @Autowired
    private Environment environment;

    public void doSome(){
        // 直接使用这个环境对象，来获取环境信息，配置信息等。
        String[] activeProfiles = environment.getActiveProfiles();
        for (String activeProfile : activeProfiles) {
            System.out.println(activeProfile);
        }

        // 获取配置信息
        String street = environment.getProperty("app.xyz.addr.street");
        System.out.println(street);
    }
}

```

通过这种方式，你可以根据环境的不同灵活地配置你的应用程序。`Environment`是一个非常有用的工具，它可以帮助你管理各种类型的配置信息，并根据不同的运行时条件做出相应的调整。


