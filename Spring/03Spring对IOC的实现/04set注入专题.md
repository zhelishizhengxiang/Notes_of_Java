注：set注入在实际使用中，用到的较多。所以set注入作为一个专题来学习。**set注入可以实现的功能，构造注入同样可以使用，只不过使用的标签不同罢了**

### 1.注入内部Bean和外部Bean

#### 1.1注入外部Bean

**注入外部Bean：Bean定义在外面，在property标签中使用ref属性来注入该Bean。==通常这种方式是常用**==
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

   <!--声明/定义全局Bean-->  
	<bean id="orderDaoBean" class="com.powernode.spring6.dao.OrderDao"></bean>  
	  
	<bean id="orderServiceBean" class="com.powernode.spring6.service.OrderService">  
		<!--使用ref属性来引入。这就是注入外部Bean-->  
		<property name="orderDao" ref="orderDaoBean"/>  
	</bean>

</beans>
```

### 1.2注入内部Bean

**注入外部Bean：在bean标签中嵌套bean标签。**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userServiceBean" class="com.powernode.spring6.service.UserService">
        <property name="userDao">
	        <!--在property标签中使用嵌套的bean标签，这就是内部Bean-->
            <bean class="com.powernode.spring6.dao.UserDao"/>
        </property>
    </bean>

</beans>
```
![](assets/04set注入专题/file-20250723201042241.png)

此种方式作为了解，不经常用
