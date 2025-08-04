
## 一、事务概述

- 什么是事务
   - 在一个业务流程当中，通常需要多条DML（insert delete update）语句共同联合才能完成，这多条DML语句必须同时成功，或者同时失败，这样才能保证数据的安全。
   - 多条DML要么同时成功，要么同时失败，这叫做事务。
   - 事务：Transaction（tx）
- 事务的四个处理过程：
   - 第一步：开启事务 (start transaction)
   - 第二步：执行核心业务代码
   - 第三步：提交事务（如果核心业务处理过程中没有出现异常）(commit transaction)
   - 第四步：回滚事务（如果核心业务处理过程中出现异常）(rollback transaction)
- 事务的四个特性：
   - A 原子性：事务是最小的工作单元，不可再分。
   - C 一致性：事务要求要么同时成功，要么同时失败。事务前和事务后的总量不变。
   - I 隔离性：事务和事务之间因为有隔离性，才可以保证互不干扰。
   - D 持久性：持久性是事务结束的标志。


## 二、引入事务场景
以银行账户转账为例学习事务。两个账户act-001和act-002。act-001账户向act-002账户转账10000，必须同时成功，或者同时失败。（一个减成功，一个加成功， 这两条update语句必须同时成功，或同时失败。）

连接数据库的技术采用Spring框架的JdbcTemplate。

采用三层架构搭建：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666495641174-069ee06f-097c-4f44-9a29-ca3e701d666b.png#averageHue=%23f7f8f5&clientId=uae187c3b-e934-4&from=paste&height=366&id=u78262625&originHeight=508&originWidth=919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99941&status=done&style=shadow&taskId=u091a372f-ef95-48b9-8fda-dcae80e1468&title=&width=663)
模块名：spring6-013-tx-bank（依赖如下）
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.powernode</groupId>
    <artifactId>spring6-013-tx-bank</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <!--仓库-->
    <repositories>
        <!--spring里程碑版本的仓库-->
        <repository>
            <id>repository.spring.milestone</id>
            <name>Spring Milestone Repository</name>
            <url>https://repo.spring.io/milestone</url>
        </repository>
    </repositories>

    <!--依赖-->
    <dependencies>
        <!--spring context-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--spring jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.30</version>
        </dependency>
      <!--德鲁伊连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.13</version>
        </dependency>
      <!--@Resource注解-->
        <dependency>
            <groupId>jakarta.annotation</groupId>
            <artifactId>jakarta.annotation-api</artifactId>
            <version>2.1.1</version>
        </dependency>
        <!--junit-->
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


### 第一步：准备数据库表
表结构：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666496097440-75d21db2-588b-4f6a-bd40-149c3de6f27d.png#averageHue=%23f5f3f2&clientId=uae187c3b-e934-4&from=paste&height=183&id=ue8b0909d&originHeight=183&originWidth=792&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18901&status=done&style=shadow&taskId=ubf854639-fb59-4c00-b536-55f060658ad&title=&width=792)
表数据：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666496136146-5cc1d848-0ad4-425d-a1fc-8b59b5d0b91f.png#averageHue=%23f3f2f0&clientId=uae187c3b-e934-4&from=paste&height=134&id=u2efb26dc&originHeight=134&originWidth=339&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7935&status=done&style=shadow&taskId=uff2b8130-c329-47be-b8a7-d27d204ff1c&title=&width=339)
### 第二步：创建包结构
com.powernode.bank.pojo
com.powernode.bank.service
com.powernode.bank.service.impl
com.powernode.bank.dao
com.powernode.bank.dao.impl

### 第三步：准备POJO类
```java
package com.powernode.bank.pojo;

/**
 * @author 动力节点
 * @version 1.0
 * @className Account
 * @since 1.0
 **/
public class Account {
    private String actno;
    private Double balance;

    @Override
    public String toString() {
        return "Account{" +
                "actno='" + actno + '\'' +
                ", balance=" + balance +
                '}';
    }

    public Account() {
    }

    public Account(String actno, Double balance) {
        this.actno = actno;
        this.balance = balance;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }
}

```

### 第四步：编写持久层
```java
package com.powernode.bank.dao;

import com.powernode.bank.pojo.Account;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountDao
 * @since 1.0
 **/
public interface AccountDao {

    /**
     * 根据账号查询余额
     * @param actno
     * @return
     */
    Account selectByActno(String actno);

    /**
     * 更新账户
     * @param act
     * @return
     */
    int update(Account act);

}

```
```java
package com.powernode.bank.dao.impl;

import com.powernode.bank.dao.AccountDao;
import com.powernode.bank.pojo.Account;
import jakarta.annotation.Resource;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountDaoImpl
 * @since 1.0
 **/
@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {

    @Resource(name = "jdbcTemplate")
    private JdbcTemplate jdbcTemplate;

    @Override
    public Account selectByActno(String actno) {
        String sql = "select actno, balance from t_act where actno = ?";
        Account account = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Account.class), actno);
        return account;
    }

    @Override
    public int update(Account act) {
        String sql = "update t_act set balance = ? where actno = ?";
        int count = jdbcTemplate.update(sql, act.getBalance(), act.getActno());
        return count;
    }
}

```

### 第五步：编写业务层
```java
package com.powernode.bank.service;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountService
 * @since 1.0
 **/
public interface AccountService {

    /**
     * 转账
     * @param fromActno
     * @param toActno
     * @param money
     */
    void transfer(String fromActno, String toActno, double money);
}

```
```java
package com.powernode.bank.service.impl;

import com.powernode.bank.dao.AccountDao;
import com.powernode.bank.pojo.Account;
import com.powernode.bank.service.AccountService;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountServiceImpl
 * @since 1.0
 **/
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Resource(name = "accountDao")
    private AccountDao accountDao;

    @Override
    public void transfer(String fromActno, String toActno, double money) {
        // 查询账户余额是否充足
        Account fromAct = accountDao.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new RuntimeException("账户余额不足");
        }
        // 余额充足，开始转账
        Account toAct = accountDao.selectByActno(toActno);
        fromAct.setBalance(fromAct.getBalance() - money);
        toAct.setBalance(toAct.getBalance() + money);
        int count = accountDao.update(fromAct);
        count += accountDao.update(toAct);
        if (count != 2) {
            throw new RuntimeException("转账失败，请联系银行");
        }
    }
}

```


### 第六步：编写Spring配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

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

</beans>
```

### 第七步：编写表示层（测试程序）
```java
package com.powernode.spring6.test;

import com.powernode.bank.service.AccountService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author 动力节点
 * @version 1.0
 * @className BankTest
 * @since 1.0
 **/
public class BankTest {
    @Test
    public void testTransfer(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        AccountService accountService = applicationContext.getBean("accountService", AccountService.class);
        try {
            accountService.transfer("act-001", "act-002", 10000);
            System.out.println("转账成功");
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}

```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666497683531-b14430f2-b90e-4555-8552-1de9747c9fcc.png#averageHue=%23f7f4f3&clientId=uae187c3b-e934-4&from=paste&height=167&id=u3b1a7771&originHeight=167&originWidth=545&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18404&status=done&style=shadow&taskId=u37ae7808-00d8-4639-ae1c-21a20cebd45&title=&width=545)
数据变化：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666497727323-b2ca34c9-99c6-4b23-8d3b-8dbe3009d3e9.png#averageHue=%23f4f3f1&clientId=uae187c3b-e934-4&from=paste&height=146&id=u349bac3e&originHeight=146&originWidth=366&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8216&status=done&style=shadow&taskId=ua8cb3724-50c9-4fe1-884c-aefbbab9044&title=&width=366)


### 模拟异常
```java
package com.powernode.bank.service.impl;

import com.powernode.bank.dao.AccountDao;
import com.powernode.bank.pojo.Account;
import com.powernode.bank.service.AccountService;
import jakarta.annotation.Resource;
import org.springframework.stereotype.Service;

/**
 * @author 动力节点
 * @version 1.0
 * @className AccountServiceImpl
 * @since 1.0
 **/
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Resource(name = "accountDao")
    private AccountDao accountDao;

    @Override
    public void transfer(String fromActno, String toActno, double money) {
        // 查询账户余额是否充足
        Account fromAct = accountDao.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new RuntimeException("账户余额不足");
        }
        // 余额充足，开始转账
        Account toAct = accountDao.selectByActno(toActno);
        fromAct.setBalance(fromAct.getBalance() - money);
        toAct.setBalance(toAct.getBalance() + money);
        int count = accountDao.update(fromAct);
        
        // 模拟异常
        String s = null;
        s.toString();

        count += accountDao.update(toAct);
        if (count != 2) {
            throw new RuntimeException("转账失败，请联系银行");
        }
    }
}

```
执行结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666497808309-c50af959-1a57-480c-9f31-76f6ce3b555a.png#averageHue=%23f9f5f3&clientId=uae187c3b-e934-4&from=paste&height=222&id=ufadbc5cf&originHeight=222&originWidth=1121&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50039&status=done&style=shadow&taskId=uddcc7f27-6fa7-478e-be1f-763193c1efe&title=&width=1121)
数据库表中数据：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666497824308-bdd8f11f-8f99-4195-81c4-c37721627f4c.png#averageHue=%23f2f1ef&clientId=uae187c3b-e934-4&from=paste&height=136&id=u953308f7&originHeight=136&originWidth=298&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7521&status=done&style=shadow&taskId=ud299b8c4-ea8d-485c-b51f-60a57ab6d2b&title=&width=298)
**丢了1万。**


