
* 缓存：cache  
* 缓存的作用：通过减少IO的方式，来提高程序的执行效率。  
* **mybatis的缓存：将select语句的查询结果放到缓存（内存）当中，下一次还是这条select语句的话，直接从缓存中取，不再查数据库。一方面是减少了IO。另一方面不再执行繁琐的查找算法。效率大大提升。**
* mybatis缓存包括：
	- **一级缓存：将查询到的数据存储到SqlSession中**。
	- **二级缓存：将查询到的数据存储到SqlSessionFactory中**。
	- 或者集成其它第三方的缓存：比如EhCache【Java语言开发的】、Memcache【C语言开发的】等。  
* **缓存只针对于DQL语句，也就是说缓存机制只对应select语句。**
## 14.1 一级缓存
* **一级缓存默认是开启的。不需要做任何配置**。
* **原理：只要使用同一个SqlSession对象执行同一条SQL语句，就会走缓存**。

模块名：mybatis-010-cache
```java
package com.powernode.mybatis.mapper;

import com.powernode.mybatis.pojo.Car;

public interface CarMapper {

    /**
     * 根据id获取Car信息。
     * @param id
     * @return
     */
    Car selectById(Long id);
}

```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.CarMapper">

  <select id="selectById" resultType="Car">
    select * from t_car where id = #{id}
  </select>

</mapper>
```
```java
package com.powernode.mybatis.test;

import com.powernode.mybatis.mapper.CarMapper;
import com.powernode.mybatis.pojo.Car;
import com.powernode.mybatis.utils.SqlSessionUtil;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

public class CarMapperTest {

    @Test
    public void testSelectById() throws Exception{
        // 注意：不能使用我们封装的SqlSessionUtil工具类。
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = builder.build(Resources.getResourceAsStream("mybatis-config.xml"));

        SqlSession sqlSession1 = sqlSessionFactory.openSession();

        CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
        Car car1 = mapper1.selectById(83L);
        System.out.println(car1);

        CarMapper mapper2 = sqlSession1.getMapper(CarMapper.class);
        Car car2 = mapper2.selectById(83L);
        System.out.println(car2);

        SqlSession sqlSession2 = sqlSessionFactory.openSession();

        CarMapper mapper3 = sqlSession2.getMapper(CarMapper.class);
        Car car3 = mapper3.selectById(83L);
        System.out.println(car3);

        CarMapper mapper4 = sqlSession2.getMapper(CarMapper.class);
        Car car4 = mapper4.selectById(83L);
        System.out.println(car4);

    }
}

```
执行结果：   
![79E534E9-50A8-4e62-BB88-C459ACD1B997.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1661154607492-3eba8947-5dda-4562-b156-2d3fe63b12a0.png#clientId=ubeb36f5f-42dd-4&from=paste&height=391&id=u974ece84&originHeight=391&originWidth=1092&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69477&status=done&style=none&taskId=u6231991d-d8a1-41ef-8812-98080ab4e7b&title=&width=1092)
1. 什么情况下不走缓存？
	- **第一种：不同的SqlSession对象。**
	- **第二种：查询条件变化了**。
2. **一级缓存失效情况包括两种**：  
- 第一种：第一次查询和第二次查询之间，手动清空了一级缓存。
```java
sqlSession.clearCache();
```

* **第二种：第一次查询和第二次查询之间，执行了增删改操作。【这个增删改和哪张表没有关系，只要有insert delete update操作，一级缓存就失效。**】
```java
/**
* 保存账户信息
*/
void insertAccount();
```
```xml
<insert id="insertAccount">
  insert into t_act values(3, 'act003', 10000)
</insert>
```
![87026BA7-38F7-41c1-86B4-8462801F93BF.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1661155578490-1b1d260d-991a-44ef-8c94-ba68796c7f03.png#clientId=ubeb36f5f-42dd-4&from=paste&height=852&id=ua9306740&originHeight=852&originWidth=1133&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88687&status=done&style=none&taskId=u8e6dacbe-ad3b-49f6-9174-ac1be7a67b7&title=&width=1133)
执行结果：  
![CBCDA9A8-3949-4bd0-8D10-56888A4286C3.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1661155640234-bdba6b74-80cf-4604-8185-fd504994150d.png#clientId=ubeb36f5f-42dd-4&from=paste&height=356&id=u7ceb82d2&originHeight=356&originWidth=1047&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54381&status=done&style=none&taskId=ubf72548b-7e6e-4e18-8c2a-b7dd2c30589&title=&width=1047)


## 14.2 二级缓存
**二级缓存的范围是SqlSessionFactory**。

**使用二级缓存需要具备以下几个条件**：
1. \<setting name="cacheEnabled" value="true"> 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。**默认就是true，无需设置**。
2. **在需要使用二级缓存的SqlMapper.xml文件中添加配置：<cache />**
3. **使用二级缓存的实体类对象必须是可序列化的，也就是必须实现java.io.Serializable接口**
4. **SqlSession对象关闭或提交之后，一级缓存中的数据才会被写入到二级缓存当中。此时二级缓存才可用**。

测试二级缓存：
```xml
<cache/>
```
```java
public class Car implements Serializable {
//......
}
```
```java
@Test
public void testSelectById2() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));

    SqlSession sqlSession1 = sqlSessionFactory.openSession();
    CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
    Car car1 = mapper1.selectById(83L);
    System.out.println(car1);

    // 关键一步
    sqlSession1.close();

    SqlSession sqlSession2 = sqlSessionFactory.openSession();
    CarMapper mapper2 = sqlSession2.getMapper(CarMapper.class);
    Car car2 = mapper2.selectById(83L);
    System.out.println(car2);
}
```
![4B715968-A13F-4fee-B41B-604504A1D847.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1661157492039-a04066a4-a824-4125-8533-e323bfd8bfdb.png#clientId=ubeb36f5f-42dd-4&from=paste&height=456&id=uf5c71836&originHeight=456&originWidth=1197&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85747&status=done&style=none&taskId=u001bd45e-7269-4f20-8895-37f7c9f2068&title=&width=1197)

* **二级缓存的失效：只要两次查询之间出现了增删改操作。二级缓存就会失效。【一级缓存也会失效】**


**二级缓存的相关配置：**
![F7BACAAD-5DD9-43e8-A2A5-5AC787BAFFB4.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1661158385819-8074adeb-f769-48f5-8519-a79a515e8631.png#clientId=ubeb36f5f-42dd-4&from=paste&height=294&id=u7f47bc31&originHeight=294&originWidth=667&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24147&status=done&style=none&taskId=u7ec00183-f3f3-465e-abf6-c9edfafcc54&title=&width=667)

1. eviction：指定从缓存中移除某个对象的淘汰算法。默认采用LRU策略。
   2. LRU：Least Recently Used。最近最少使用。优先淘汰在间隔时间内使用频率最低的对象。(其实还有一种淘汰算法LFU，最不常用。)
   3. FIFO：First In First Out。一种先进先出的数据缓存器。先进入二级缓存的对象最先被淘汰。
   4. SOFT：软引用。淘汰软引用指向的对象。具体算法和JVM的垃圾回收算法有关。
   5. WEAK：弱引用。淘汰弱引用指向的对象。具体算法和JVM的垃圾回收算法有关。
6. flushInterval：
   7. 二级缓存的刷新时间间隔。单位毫秒。如果没有设置。就代表不刷新缓存，只要内存足够大，一直会向二级缓存中缓存数据。除非执行了增删改。
8. readOnly：
   9. true：多条相同的sql语句执行之后返回的对象是共享的同一个。性能好。但是多线程并发可能会存在安全问题。
   10. false：多条相同的sql语句执行之后返回的对象是副本，调用了clone方法。性能一般。但安全。
11. size：
   12. 设置二级缓存中最多可存储的java对象数量。默认值1024。
## 14.3 MyBatis集成EhCache
集成EhCache是为了代替mybatis自带的二级缓存。一级缓存是无法替代的。
mybatis对外提供了接口，也可以集成第三方的缓存组件。比如EhCache、Memcache等。都可以。
EhCache是Java写的。Memcache是C语言写的。所以mybatis集成EhCache较为常见，按照以下步骤操作，就可以完成集成：
第一步：引入mybatis整合ehcache的依赖。
```xml
<!--mybatis集成ehcache的组件-->
<dependency>
  <groupId>org.mybatis.caches</groupId>
  <artifactId>mybatis-ehcache</artifactId>
  <version>1.2.2</version>
</dependency>
```
第二步：在类的根路径下新建echcache.xml文件，并提供以下配置信息。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--磁盘存储:将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存-->
    <diskStore path="e:/ehcache"/>
  
    <!--defaultCache：默认的管理策略-->
    <!--eternal：设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断-->
    <!--maxElementsInMemory：在内存中缓存的element的最大数目-->
    <!--overflowToDisk：如果内存中数据超过内存限制，是否要缓存到磁盘上-->
    <!--diskPersistent：是否在磁盘上持久化。指重启jvm后，数据是否有效。默认为false-->
    <!--timeToIdleSeconds：对象空闲时间(单位：秒)，指对象在多长时间没有被访问就会失效。只对eternal为false的有效。默认值0，表示一直可以访问-->
    <!--timeToLiveSeconds：对象存活时间(单位：秒)，指对象从创建到失效所需要的时间。只对eternal为false的有效。默认值0，表示一直可以访问-->
    <!--memoryStoreEvictionPolicy：缓存的3 种清空策略-->
    <!--FIFO：first in first out (先进先出)-->
    <!--LFU：Less Frequently Used (最少使用).意思是一直以来最少被使用的。缓存的元素有一个hit 属性，hit 值最小的将会被清出缓存-->
    <!--LRU：Least Recently Used(最近最少使用). (ehcache 默认值).缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存-->
    <defaultCache eternal="false" maxElementsInMemory="1000" overflowToDisk="false" diskPersistent="false"
                  timeToIdleSeconds="0" timeToLiveSeconds="600" memoryStoreEvictionPolicy="LRU"/>

</ehcache>
```
第三步：修改SqlMapper.xml文件中的<cache/>标签，添加type属性。
```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```
第四步：编写测试程序使用。
```java
@Test
public void testSelectById2() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    
    SqlSession sqlSession1 = sqlSessionFactory.openSession();
    CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
    Car car1 = mapper1.selectById(83L);
    System.out.println(car1);
    
    sqlSession1.close();
    
    SqlSession sqlSession2 = sqlSessionFactory.openSession();
    CarMapper mapper2 = sqlSession2.getMapper(CarMapper.class);
    Car car2 = mapper2.selectById(83L);
    System.out.println(car2);
}
```
![509D28EA-B111-4151-AFC3-E6D3C3BB3F52.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1661162595603-3aeecedf-d1f5-4b53-bd76-ee3c52f014fd.png#clientId=ubeb36f5f-42dd-4&from=paste&height=400&id=ua7ee1609&originHeight=400&originWidth=1129&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70384&status=done&style=none&taskId=u14da606c-a1cb-42e2-babd-184e7bab9e4&title=&width=1129)
