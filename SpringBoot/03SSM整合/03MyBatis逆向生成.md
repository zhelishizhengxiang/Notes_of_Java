
**MyBatis逆向工程：使用IDEA插件可以根据数据库表的设计逆向生成MyBatis的Mapper接口 与 MapperXML文件。**

## 1.安装插件`free mybatis tools`
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729234849712-989025f9-e324-45c0-a126-48ea9186582b.png)



## 2.在IDEA中配置数据源
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729234975933-38892ecf-5c92-4626-904b-223426f8f026.png)



## 生成MyBatis代码放到SpringBoot项目中
在表上右键：Mybatis-Generator

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729235071633-a4d6bd7a-dc80-45c3-bc52-31dfee5789bf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729235692907-6637331b-7b98-41d0-9ca2-47ed44c9bab0.png)


代码生成后，如果在IDEA中看不到，这样做（重新从硬盘加载）：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729235782802-6c83273c-0b14-405f-a1c8-793fc80c9123.png)



**<font style="color:#DF2A3F;">注意：生成的</font>**`**<font style="color:#DF2A3F;">VipMapper</font>**`**<font style="color:#DF2A3F;">接口上自动添加了</font>**`**<font style="color:#DF2A3F;">@Repository</font>**`**<font style="color:#DF2A3F;">注解，这个注解没用，删除即可。</font>**

**<font style="color:#DF2A3F;"></font>**


## 编写mybatis相关配置
application.properties属性文件的配置：

```properties
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=root
spring.datasource.password=123456

mybatis.mapper-locations=classpath:com/powernode/springboot/repository/*.xml
mybatis.configuration.map-underscore-to-camel-case=true
```


## 编写测试程序
```java
package com.powernode.springboot;

import com.powernode.springboot.model.Vip;
import com.powernode.springboot.repository.VipMapper;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@MapperScan(basePackages = "com.powernode.springboot.repository")
@SpringBootApplication
public class Sb306SpringbootMybatisGeneratorApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(Sb306SpringbootMybatisGeneratorApplication.class, args);
        VipMapper vipMapper = applicationContext.getBean("vipMapper", VipMapper.class);
        // 增
        Vip vip = new Vip();
        vip.setName("孙悟空");
        vip.setBirth("1999-11-11");
        vip.setCardNumber("1234567894");
        vipMapper.insert(vip);
        // 查一个
        Vip vip1 = vipMapper.selectByPrimaryKey(2L);
        System.out.println(vip1);
        // 改
        vip1.setName("孙行者");
        vipMapper.updateByPrimaryKey(vip1);
        // 删
        vipMapper.deleteByPrimaryKey(1L);

        // 关闭Spring容器
        applicationContext.close();
    }
}
```

到此，Spring Boot整合MyBatis结束！



