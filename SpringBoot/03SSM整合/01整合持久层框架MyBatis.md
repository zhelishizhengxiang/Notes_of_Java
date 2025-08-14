 

## 1.准备数据库表及数据
创建数据库：springboot

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729050319150-d942131f-2eb9-4baa-870f-3d15f4cd7479.png)

使用IDEA工具自带的mysql插件来完成表的创建和数据的准备：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729050185616-731cbd39-267f-45e1-81f3-1f07d621c514.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729050390076-57de6c2a-36c7-402f-8bc3-e4e4cb0d50f3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729050451224-c17b4676-0020-418d-80f9-f24738427fc6.png)



![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729051111940-4a591196-a5d9-48c2-83b7-94d858595561.png)





表创建成功后，为表准备数据，如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729051234644-3deae02b-9aec-4017-8dbc-5f3337c5c179.png)


## 2.创建SpringBoot项目
使用脚手架创建Spring Boot项目

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729049362956-f6fc7e25-ef28-40dd-abd8-06c69a2649a5.png)



引入mysql驱动以及mybatis的启动器

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729049442355-3e02c359-f9a7-4afb-93ec-0a314475c882.png)



依赖如下：

```xml
<!--mybatis的启动器-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>3.0.3</version>
</dependency>
<!--mysql的驱动依赖-->
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>
```
## 3.编写数据源配置
前面提到过，Spring Boot配置统一可以编写到application.properties中，配置如下：

```properties
# Spring Boot脚手架自动生成的
spring.application.name=sb3-05-springboot-mybatis

# mybatis连接数据库的数据源
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=root
spring.datasource.password=123456
```

以上的配置属于连接池的配置，连接池使用的是Spring Boot默认的连接池：HikariCP



## 4.编写实体类Vip
表`t_vip`中的字段分别是：

+ id
+ name
+ card_number
+ birth

对应实体类`Vip`中的属性名分别是：

+ Long id;
+ String name;
+ String cardNumber;
+ String birth;

创建包`model`，在该包下新建Vip类，代码如下：

```java
package com.powernode.sb305springbootmybatis.model;

public class Vip {
    private Long id;
    private String name;
    private String cardNumber;
    private String birth;

    public Vip() {
    }

    public Vip(Long id, String name, String cardNumber, String birth) {
        this.id = id;
        this.name = name;
        this.cardNumber = cardNumber;
        this.birth = birth;
    }

    public Vip(String name, String cardNumber, String birth) {
        this.name = name;
        this.cardNumber = cardNumber;
        this.birth = birth;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCardNumber() {
        return cardNumber;
    }

    public void setCardNumber(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    public String getBirth() {
        return birth;
    }

    public void setBirth(String birth) {
        this.birth = birth;
    }

    @Override
    public String toString() {
        return "Vip{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", cardNumber='" + cardNumber + '\'' +
                ", birth='" + birth + '\'' +
                '}';
    }
}
```

以上代码可以使用第三方库Lombok进行改造，后面再说。


## 5.编写Mapper接口
创建`repository`包，在该包下新建`VipMapper`接口，代码如下：

```java
package com.powernode.sb305springbootmybatis.repository;

import com.powernode.sb305springbootmybatis.model.Vip;

import java.util.List;

public interface VipMapper {
    /**
     * 插入会员信息
     * @param vip
     * @return 1表示插入成功，其他值表示失败
     */
    int insert(Vip vip);

    /**
     * 根据id删除会员信息
     * @param id 会员唯一标识
     * @return 1表示删除成功，其他值表示失败
     */
    int deleteById(Long id);

    /**
     * 更新会员信息（id不可更新）
     * @param vip 会员信息
     * @return 1表示更新成功，其他值表示更新失败。
     */
    int update(Vip vip);

    /**
     * 根据id查询会员信息
     * @param id 会员的唯一标识
     * @return 会员信息
     */
    Vip selectById(Long id);

    /**
     * 获取所有会员信息
     * @return
     */
    List<Vip> selectAll();
}
```



## 6.编写Mapper接口的XML配置文件
在`resources`目录下新建`mapper`目录，将来的`mapper.xml`配置文件放在这个目录下。



安装`MyBatisX`插件，该插件可以根据我们编写的`VipMapper`接口自动生成mapper的XML配置文件。

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729132285817-67182b8c-487e-4ef5-b061-da4b12174489.png)



然后在`VipMapper`接口上：alt+enter

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729132441611-d68a935b-4a8a-4408-998f-a1cdbf50fe99.png)

生成`mapper of xml`：需要选择一个生成的位置

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729132515796-a056a2c3-e464-4c6e-bf8c-c1a876e36b80.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729132546447-714f679f-9b8c-4482-9e8d-d1c7e525a3c5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729132560250-b0ee37ad-8954-4b5d-925e-cc0cb5e14040.png)

接下来，你会看到Mapper接口中方法报错了，可以在错误的位置上使用`alt+enter`，选择`Generate statement`：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729132763164-68d5a9b0-76ea-43fb-b89d-01050e82c4e6.png)

这个时候在mapper的xml配置文件中便生成了对应的配置，如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729133020043-c289aa19-ba87-4996-b18a-6411779eba6d.png)

接下来就是编写SQL语句了，最终`VipMapper.xml`文件的配置如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.powernode.sb305springbootmybatis.repository.VipMapper">
    <insert id="insert">
        insert into t_vip(id,name,card_number,birth) values(null,#{name},#{cardNumber},#{birth})
    </insert>
    <update id="update">
        update t_vip set name=#{name},card_number=#{cardNumber},birth=#{birth} where id=#{id}
    </update>
    <delete id="deleteById">
        delete from t_vip where id = #{id}
    </delete>
    <select id="selectById" resultType="com.powernode.sb305springbootmybatis.model.Vip">
        select * from t_vip where id=#{id}
    </select>
    <select id="selectAll" resultType="com.powernode.sb305springbootmybatis.model.Vip">
        select * from t_vip
    </select>
</mapper>
```



## 7.添加Mapper的扫描
**在Spring Boot的入口程序上添加如下的注解，来完成`VipMapper`接口的扫描：**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729133412840-754cf9d1-1f3c-4667-bf7b-09a0018f7581.png)



## 8.告诉MyBatis框架MapperXML文件的位置
在`application.properties`配置文件中进行如下配置：

```properties
mybatis.mapper-locations=classpath:mapper/*.xml
```

## 9.测试整合MyBatis是否成功
在Spring Boot主入口程序中获取Spring上下文对象`ApplicationContext`，从Spring容器中获取`VipMapper`对象，然后调用相关方法进行测试：

```java
package com.powernode.sb305springbootmybatis;

import com.powernode.sb305springbootmybatis.model.Vip;
import com.powernode.sb305springbootmybatis.repository.VipMapper;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@MapperScan(basePackages = {"com.powernode.sb305springbootmybatis.repository"})
@SpringBootApplication
public class Sb305SpringbootMybatisApplication {

    public static void main(String[] args) {
        // 获取Spring上下文
        ConfigurableApplicationContext applicationContext = SpringApplication.run(Sb305SpringbootMybatisApplication.class, args);
        // 根据id获取容器中的对象
        VipMapper vipMapper = applicationContext.getBean("vipMapper", VipMapper.class);
        Vip vip = vipMapper.selectById(1L);
        System.out.println(vip);
        // 关闭Spring上下文
        applicationContext.close();
    }

}

```



测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729135284617-032c08b9-07f8-4d73-ba82-4529aac1f2fb.png)



测试结果中可以看到`cardNumber`属性没有赋值成功，原因是：表中的字段名叫做`card_number`，和实体类`Vip`的属性名`cardNumber`对应不上。解决办法两个：

+ **第一种方式：查询语句使用as关键字起别名，让查询结果列名和实体类的属性名对应上。**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729135489213-4e91d0d1-17f7-48fd-ad1c-568443cd78e0.png)

再次测试：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729135540950-be358277-5da0-4c5d-868f-075cd2ed8ea0.png)


+ **第二种方式：通过配置自动映射**

在`application.properties`配置文件中进行如下配置：

```properties
mybatis.configuration.map-underscore-to-camel-case=true
```

map-underscore-to-camel-case 是一个配置项，主要用于处理数据库字段名与Java对象属性名之间的命名差异。在许多数据库中，字段名通常使用下划线（_）分隔单词，例如 first_name 或 last_name。而在Java代码中，变量名通常使用驼峰式命名法（camel case），如 firstName 和 lastName。

当使用MyBatis作为ORM框架时，默认情况下它会将SQL查询结果映射到Java对象的属性上。如果数据库中的字段名与Java对象的属性名不一致，那么就需要手动为每个字段指定相应的属性名，或者使用某种方式来自动转换这些名称。

map-underscore-to-camel-case 这个配置项的作用就是在查询结果映射到Java对象时，自动将下划线分隔的字段名转换成驼峰式命名法。这样可以减少手动映射的工作量，并提高代码的可读性和可维护性。



mapper的xml文件中的sql语句仍然使用`*`的方式：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729135719372-7d2287fd-9fa1-4e8b-8558-d769417d9d9e.png)





测试结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729135946867-e2f2fda6-25fe-430c-af38-0831056571c9.png)

## 测试其他方法是否正常
测试程序如下：

```java
package com.powernode.sb305springbootmybatis;

import com.powernode.sb305springbootmybatis.model.Vip;
import com.powernode.sb305springbootmybatis.repository.VipMapper;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

import java.util.List;

@MapperScan(basePackages = {"com.powernode.sb305springbootmybatis.repository"})
@SpringBootApplication
public class Sb305SpringbootMybatisApplication {

    public static void main(String[] args) {
        // 获取Spring上下文
        ConfigurableApplicationContext applicationContext = SpringApplication.run(Sb305SpringbootMybatisApplication.class, args);
        // 根据id获取容器中的对象
        VipMapper vipMapper = applicationContext.getBean("vipMapper", VipMapper.class);
        Vip vip = vipMapper.selectById(1L);
        System.out.println(vip);
        // 添加会员信息
        Vip newVip = new Vip("杰克", "1234567892", "1999-11-10");
        vipMapper.insert(newVip);
        // 查询所有会员信息
        List<Vip> vips = vipMapper.selectAll();
        System.out.println(vips);
        // 修改会员信息
        vip.setName("zhangsan");
        vipMapper.update(vip);
        // 查询所有会员信息
        List<Vip> vips2 = vipMapper.selectAll();
        System.out.println(vips2);
        // 删除会员信息
        vipMapper.deleteById(1L);
        // 查询所有会员信息
        List<Vip> vips3 = vipMapper.selectAll();
        System.out.println(vips3);
        // 关闭Spring上下文
        applicationContext.close();
    }

}

```

执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729136373779-97569876-0b74-4774-b306-a6bbc838462e.png)



到此为止，我们已经完成了Spring Boot整合MyBatis的操作。

