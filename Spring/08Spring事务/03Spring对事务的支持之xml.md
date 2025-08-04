### 1.声明式事务之XML实现方式

配置步骤：
- 第一步：配置事务管理器
- 第二步：配置通知
- 第三步：配置切面

记得添加aspectj的依赖：
```xml
<!--aspectj依赖-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>6.0.0-M2</version>
</dependency>
```
Spring配置文件如下：
**记得添加aop的命名空间。**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <context:component-scan base-package="com.powernode.bank"/>

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring6"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--配置通知-->
    <tx:advice id="txAdvice" transaction-manager="txManager">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
            <tx:method name="del*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
            <tx:method name="update*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
            <tx:method name="transfer*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
        </tx:attributes>
    </tx:advice>

    <!--配置切面-->
    <aop:config>
        <aop:pointcut id="txPointcut" expression="execution(* com.powernode.bank.service..*(..))"/>
        <!--切面 = 通知 + 切点-->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
    </aop:config>

</beans>
```
将AccountServiceImpl类上的@Transactional注解删除。
编写测试程序：
```java
@Test
public void testTransferXml(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring2.xml");
    AccountService accountService = applicationContext.getBean("accountService", AccountService.class);
    try {
        accountService.transfer("act-001", "act-002", 10000);
        System.out.println("转账成功");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
执行结果：     
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666510211960-60399e1d-ae1c-4e73-9593-3ce0086bf143.png#averageHue=%23f8f4f2&clientId=uae187c3b-e934-4&from=paste&height=266&id=ub662fbc9&originHeight=266&originWidth=1116&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63969&status=done&style=shadow&taskId=u31cf1aa3-8722-42ee-9b00-c3192b7f968&title=&width=1116)
数据库表中记录：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666510230350-5150f5ca-3812-40d6-8817-adc102516e7e.png#averageHue=%23f4f3f2&clientId=uae187c3b-e934-4&from=paste&height=150&id=ub644a306&originHeight=150&originWidth=356&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8520&status=done&style=shadow&taskId=ue28c57cd-8f46-4649-b48f-0a8b3c694b3&title=&width=356)

通过测试可以看到配置XML已经起作用了。