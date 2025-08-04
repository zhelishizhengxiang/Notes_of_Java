**本章主要是了解spring也封装了JDBC的代码，简化开发。实际都是用一些持久层框架。所以这章了解即可**

**JdbcTemplate是Spring提供的一个JDBC模板类，是对JDBC的封装，简化JDBC代码**。

当然，你也可以不用，可以让Spring集成其它的ORM框架，例如：MyBatis、Hibernate等。
接下来我们简单来学习一下，使用JdbcTemplate完成增删改查。
## 1. 环境准备

数据库表：t_user    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665633536319-466a1b96-90ff-4a87-82ad-fb14f32a8d12.png#averageHue=%23f5f4f3&clientId=u27fdab07-dcc6-4&from=paste&height=188&id=u76c20684&originHeight=188&originWidth=800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19315&status=done&style=shadow&taskId=u8108b33a-018c-4933-a192-577b7934751&title=&width=800)
IDEA中新建模块：spring6-007-jdbc    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665633731889-aa224eae-8ff7-47af-aaf5-70500c7cf37e.png#averageHue=%23f3f2f2&clientId=u27fdab07-dcc6-4&from=paste&height=601&id=u3f985b79&originHeight=601&originWidth=777&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40078&status=done&style=shadow&taskId=u1df9c873-ae00-458c-8806-a820a25e420&title=&width=777)
引入相关依赖：  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.powernode</groupId>
    <artifactId>spring6-007-jdbc</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <repositories>
        <repository>
            <id>repository.spring.milestone</id>
            <name>Spring Milestone Repository</name>
            <url>https://repo.spring.io/milestone</url>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
        <!--新增的依赖:mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.30</version>
        </dependency>
        <!--新增的依赖：spring jdbc，这个依赖中有JdbcTemplate-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```
准备实体类：表t_user对应的实体类User。  
```java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className User
 * @since 1.0
 **/
public class User {
    private Integer id;
    private String realName;
    private Integer age;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", realName='" + realName + '\'' +
                ", age=" + age +
                '}';
    }

    public User() {
    }

    public User(Integer id, String realName, Integer age) {
        this.id = id;
        this.realName = realName;
        this.age = age;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getRealName() {
        return realName;
    }

    public void setRealName(String realName) {
        this.realName = realName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}

```
编写Spring配置文件：
* **JdbcTemplate是Spring提供好的类，这类的完整类名是org.springframework.jdbc.core.JdbcTemplate**
* 我们怎么使用这个类呢？new对象就可以了。怎么new对象，Spring最在行了。**直接将这个类配置到Spring配置文件中，纳入Bean管理即可**。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"></bean>
</beans>
```
我们来看一下这个JdbcTemplate源码：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665641540149-8f44a8b1-35b6-4c8a-bd27-f08ebd911e01.png#averageHue=%23fdfcfa&clientId=u27fdab07-dcc6-4&from=paste&height=610&id=u5bdbb672&originHeight=610&originWidth=993&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91978&status=done&style=shadow&taskId=u1a85f911-01f1-4843-9622-a53b517b2a3&title=&width=993)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665641567361-50fd782b-cea4-4ca2-9818-01696aca0eb0.png#averageHue=%23fdfbf9&clientId=u27fdab07-dcc6-4&from=paste&height=404&id=uaf0bcb51&originHeight=404&originWidth=1159&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64579&status=done&style=shadow&taskId=uf4435305-b2c6-4467-b7c8-0f1b42e6579&title=&width=1159)
* 可以看到JdbcTemplate中有一个DataSource属性，这个属性是数据源，我们都知道连接数据库需要Connection对象，而生成Connection对象是数据源负责的。所以我们需要给JdbcTemplate设置数据源属性。

所有的数据源都是要实现javax.sql.DataSource接口的。这个数据源可以自己写一个，也可以用写好的，比如：阿里巴巴的德鲁伊连接池，c3p0，dbcp等。我们这里自己先手写一个数据源。
```java
package com.powernode.spring6.jdbc;

import javax.sql.DataSource;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.SQLFeatureNotSupportedException;
import java.util.logging.Logger;

/**
 * @author 动力节点
 * @version 1.0
 * @className MyDataSource
 * @since 1.0
 **/
public class MyDataSource implements DataSource {
    // 添加4个属性
    private String driver;
    private String url;
    private String username;
    private String password;

    // 提供4个setter方法
    public void setDriver(String driver) {
        this.driver = driver;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    // 重点写怎么获取Connection对象就行。其他方法不用管。
    @Override
    public Connection getConnection() throws SQLException {
        try {
            Class.forName(driver);
            Connection conn = DriverManager.getConnection(url, username, password);
            return conn;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    @Override
    public Connection getConnection(String username, String password) throws SQLException {
        return null;
    }

    @Override
    public PrintWriter getLogWriter() throws SQLException {
        return null;
    }

    @Override
    public void setLogWriter(PrintWriter out) throws SQLException {

    }

    @Override
    public void setLoginTimeout(int seconds) throws SQLException {

    }

    @Override
    public int getLoginTimeout() throws SQLException {
        return 0;
    }

    @Override
    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
        return null;
    }

    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return null;
    }

    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return false;
    }
}

```
写完数据源，我们需要把这个数据源传递给JdbcTemplate。因为JdbcTemplate中有一个DataSource属性：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="com.powernode.spring6.jdbc.MyDataSource">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring6"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="myDataSource"/>
    </bean>
</beans>
```
到这里环境就准备好了。

## 2.JDBCTemplate的使用
### 1. 新增
编写测试程序：
```java
package com.powernode.spring6.test;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

/**
 * @author 动力节点
 * @version 1.0
 * @className JdbcTest
 * @since 1.0
 **/
public class JdbcTest {
    @Test
    public void testInsert(){
        // 获取JdbcTemplate对象
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        // 执行插入操作
        // 注意：insert delete update的sql语句，都是执行update方法。
        String sql = "insert into t_user(id,real_name,age) values(?,?,?)";
        int count = jdbcTemplate.update(sql, null, "张三", 30);
        System.out.println("插入的记录条数：" + count);
    }
}

```
* **注意：insert delete update的sql语句，都是执行update方法。**
* update方法有两个参数：  
	- **第一个参数：要执行的SQL语句。（SQL语句中可能会有占位符 ? ）**
	- **第二个参数：可变长参数，参数的个数可以是0个，也可以是多个。一般是SQL语句中有几个问号，则对应几个参数。**


### 2.修改
```java
@Test
public void testUpdate(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 执行更新操作
    String sql = "update t_user set real_name = ?, age = ? where id = ?";
    int count = jdbcTemplate.update(sql, "张三丰", 55, 1);
    System.out.println("更新的记录条数：" + count);
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665642952562-a030937f-b3f5-4018-b92e-02d25ab390d7.png#averageHue=%23f4f2f1&clientId=u27fdab07-dcc6-4&from=paste&height=107&id=ufb078681&originHeight=107&originWidth=483&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10078&status=done&style=shadow&taskId=u6474de2e-0d18-4cff-ab28-57eddf050ac&title=&width=483)


### 3.删除
```java
@Test
public void testDelete(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 执行delete
    String sql = "delete from t_user where id = ?";
    int count = jdbcTemplate.update(sql, 1);
    System.out.println("删除了几条记录：" + count);
}
```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665643115536-fb7949b8-9bcd-4d45-8032-40c3658911fd.png#averageHue=%23f2f1ef&clientId=u27fdab07-dcc6-4&from=paste&height=102&id=u9cd70054&originHeight=102&originWidth=474&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9558&status=done&style=shadow&taskId=uc6e5d8c4-1963-44ba-8754-715418696e7&title=&width=474)


### 4.查询一个对象
```java
@Test
public void testSelectOne(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 执行select
    String sql = "select id, real_name, age from t_user where id = ?";
    User user = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(User.class), 2);
    System.out.println(user);
}
```
执行结果：   
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665643791948-c5a4f422-4c49-426a-88f3-004907f696dd.png#averageHue=%23f3f2f0&clientId=u27fdab07-dcc6-4&from=paste&height=124&id=ufeed9ab1&originHeight=124&originWidth=480&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12264&status=done&style=shadow&taskId=u605f6777-09a8-4a9b-9bbb-45724b3acc4&title=&width=480)
**queryForObject用于查询单个对象，其方法三个参数**：
- **第一个参数：sql语句**
- **第二个参数：Bean属性值和数据库记录行的映射对象。在构造方法中指定映射的对象类型。**
- **第三个参数：可变长参数，给sql语句的占位符问号传值**。


### 5.查询多个对象
**使用query()方法，其参数的设置与queryForObject()相同**
```java
@Test
public void testSelectAll(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 执行select
    String sql = "select id, real_name, age from t_user";
    List<User> users = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
    System.out.println(users);
}
```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665644613140-cc029b6f-7dbe-40fa-896c-ba8674da014c.png#averageHue=%23f5f3f2&clientId=u27fdab07-dcc6-4&from=paste&height=109&id=ua5ef8f52&originHeight=109&originWidth=846&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13755&status=done&style=shadow&taskId=ud2cd8d9c-41ef-4f1e-bf97-1f45c8b14d1&title=&width=846)

## 6. 查询一个值
* **查询一个结果就用queryForObject()，查询多个对象就用query()；查询的结果如果是值，就封装的结果类型为int**
```java
@Test
public void testSelectOneValue(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 执行select
    String sql = "select count(1) from t_user";
    Integer count = jdbcTemplate.queryForObject(sql, int.class); // 这里用Integer.class也可以
    System.out.println("总记录条数：" + count);
}
```
执行结果：    
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665644735494-3aff4263-8172-4e33-b5e4-564dda88a704.png#averageHue=%23f5f4f2&clientId=u27fdab07-dcc6-4&from=paste&height=110&id=u7a66fa0a&originHeight=110&originWidth=493&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9704&status=done&style=shadow&taskId=u2b2e531b-1b4d-4fbc-a55e-478fb85d6cf&title=&width=493)


### 7.批量添加
* **批量添加和删除都使用batchUpdate()。单次删除、修改和添加都是update()。**
* **使用Object\[]数组来封装一条记录。，并将每条记录加入到一个list之中，batchUpdate()中传入sql语句和list即可**
```java
@Test
public void testAddBatch(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 批量添加
    String sql = "insert into t_user(id,real_name,age) values(?,?,?)";

    Object[] objs1 = {null, "小花", 20};
    Object[] objs2 = {null, "小明", 21};
    Object[] objs3 = {null, "小刚", 22};
    List<Object[]> list = new ArrayList<>();
    list.add(objs1);
    list.add(objs2);
    list.add(objs3);

    int[] count = jdbcTemplate.batchUpdate(sql, list);
    System.out.println(Arrays.toString(count));
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665645369060-9a473341-e4d2-4c26-bdd4-7d83fd93cc82.png#averageHue=%23f5f4f3&clientId=u27fdab07-dcc6-4&from=paste&height=118&id=u4d53536e&originHeight=118&originWidth=476&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7957&status=done&style=shadow&taskId=u79ea3a82-39ec-461b-bcf0-a8d37e01b39&title=&width=476)


### 8.批量修改
```java
@Test
public void testUpdateBatch(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 批量修改
    String sql = "update t_user set real_name = ?, age = ? where id = ?";
    Object[] objs1 = {"小花11", 10, 2};
    Object[] objs2 = {"小明22", 12, 3};
    Object[] objs3 = {"小刚33", 9, 4};
    List<Object[]> list = new ArrayList<>();
    list.add(objs1);
    list.add(objs2);
    list.add(objs3);

    int[] count = jdbcTemplate.batchUpdate(sql, list);
    System.out.println(Arrays.toString(count));
}
```
执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665645613528-7edf2796-3f40-4c6d-bedc-9022fb16d7da.png#averageHue=%23f5f4f3&clientId=u27fdab07-dcc6-4&from=paste&height=108&id=u1cdb8d08&originHeight=108&originWidth=496&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7866&status=done&style=shadow&taskId=ubd026f6d-b7c8-44dd-b28b-0fecae788ab&title=&width=496)


## 9.批量删除
```java
@Test
public void testDeleteBatch(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 批量删除
    String sql = "delete from t_user where id = ?";
    Object[] objs1 = {2};
    Object[] objs2 = {3};
    Object[] objs3 = {4};
    List<Object[]> list = new ArrayList<>();
    list.add(objs1);
    list.add(objs2);
    list.add(objs3);
    int[] count = jdbcTemplate.batchUpdate(sql, list);
    System.out.println(Arrays.toString(count));
}
```
执行结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665645815657-de657db3-cb20-4758-a40e-2c049175d89e.png#averageHue=%23f6f5f4&clientId=u27fdab07-dcc6-4&from=paste&height=122&id=u3e9e0665&originHeight=122&originWidth=490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7981&status=done&style=shadow&taskId=u3292aab1-ae6a-4cae-b03d-813f9520427&title=&width=490)


### 10.使用回调函数
* **使用回调函数，也就是PreparedStatementCallback的匿名内部类，实现回调函数doInPreparedStatement()，可以在spring中写jdbc的一些细节代码**
* **注册回调函数，当execute方法执行的时候，回调函数中的doInPreparedStatement()会被调用。**
```java
@Test
public void testCallback(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    String sql = "select id, real_name, age from t_user where id = ?";
	//// 注册回调函数，当execute方法执行的时候，回调函数中的doInPreparedStatement()会被调用。
    User user = jdbcTemplate.execute(sql, new PreparedStatementCallback<User>() {
        @Override
        public User doInPreparedStatement(PreparedStatement ps) throws SQLException, DataAccessException {
            User user = null;
            //给占位符赋值
            ps.setInt(1, 5);
            ResultSet rs = ps.executeQuery();
            if (rs.next()) {
                user = new User();
                user.setId(rs.getInt("id"));
                user.setRealName(rs.getString("real_name"));
                user.setAge(rs.getInt("age"));
            }
            return user;
        }
    });
    System.out.println(user);
}
```
执行结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665646365875-6ff081a4-74a0-469d-a3ed-b579235743ee.png#averageHue=%23f3f2f0&clientId=u27fdab07-dcc6-4&from=paste&height=115&id=u0f03c359&originHeight=115&originWidth=488&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11901&status=done&style=shadow&taskId=u8dda3d94-2511-4db0-9c37-449bac390e1&title=&width=488)


## 3.使用德鲁伊连接池
之前数据源是用我们自己写的。也可以使用别人写好的。例如比较牛的德鲁伊连接池。
第一步：引入德鲁伊连接池的依赖。（毕竟是别人写的）
```xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.8</version>
</dependency>
```
第二步：将德鲁伊中的数据源配置到spring配置文件中。和配置我们自己写的一样。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring6"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
</beans>
```
测试结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665647176481-660d65ae-f65a-4448-a34d-81ec96ee3b08.png#averageHue=%23f9f6f5&clientId=u27fdab07-dcc6-4&from=paste&height=174&id=uf518af07&originHeight=174&originWidth=1017&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24562&status=done&style=shadow&taskId=uc49a8ec5-431f-4ab5-9059-2fe235736eb&title=&width=1017)

