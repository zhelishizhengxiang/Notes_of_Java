### 1.AOP的实际案例：事务处理
项目中的事务控制是在所难免的。在一个业务流程当中，可能需要多条DML语句共同完成，为了保证数据的安全，这多条DML语句要么同时成功，要么同时失败。这就需要添加事务控制的代码。例如以下伪代码：
```java
class 业务类1{
    public void 业务方法1(){
        try{
            // 开启事务
            startTransaction();
            
            // 执行核心业务逻辑
            step1();
            step2();
            step3();
            ....
            
            // 提交事务
            commitTransaction();
        }catch(Exception e){
            // 回滚事务
            rollbackTransaction();
        }
    }
    public void 业务方法2(){
        try{
            // 开启事务
            startTransaction();
            
            // 执行核心业务逻辑
            step1();
            step2();
            step3();
            ....
            
            // 提交事务
            commitTransaction();
        }catch(Exception e){
            // 回滚事务
            rollbackTransaction();
        }
    }
    public void 业务方法3(){
        try{
            // 开启事务
            startTransaction();
            
            // 执行核心业务逻辑
            step1();
            step2();
            step3();
            ....
            
            // 提交事务
            commitTransaction();
        }catch(Exception e){
            // 回滚事务
            rollbackTransaction();
        }
    }
}


class 业务类2{
    public void 业务方法1(){
        try{
            // 开启事务
            startTransaction();
            
            // 执行核心业务逻辑
            step1();
            step2();
            step3();
            ....
            
            // 提交事务
            commitTransaction();
        }catch(Exception e){
            // 回滚事务
            rollbackTransaction();
        }
    }
    public void 业务方法2(){
        try{
            // 开启事务
            startTransaction();
            
            // 执行核心业务逻辑
            step1();
            step2();
            step3();
            ....
            
            // 提交事务
            commitTransaction();
        }catch(Exception e){
            // 回滚事务
            rollbackTransaction();
        }
    }
    public void 业务方法3(){
        try{
            // 开启事务
            startTransaction();
            
            // 执行核心业务逻辑
            step1();
            step2();
            step3();
            ....
            
            // 提交事务
            commitTransaction();
        }catch(Exception e){
            // 回滚事务
            rollbackTransaction();
        }
    }
}
//......
```
可以看到，这些业务类中的每一个业务方法都是需要控制事务的，而控制事务的代码又是固定的格式，都是：
```java
try{
    // 开启事务
    startTransaction();

    // 执行核心业务逻辑
    //......

    // 提交事务
    commitTransaction();
}catch(Exception e){
    // 回滚事务
    rollbackTransaction();
}
```
这个控制事务的代码就是和业务逻辑没有关系的“**交叉业务**”。以上伪代码当中可以看到这些交叉业务的代码没有得到复用，并且如果这些交叉业务代码需要修改，那必然需要修改多处，难维护，怎么解决？**可以采用AOP思想解决。可以把以上控制事务的代码作为环绕通知，切入到目标类的方法当中**。接下来我们做一下这件事，有两个业务类，如下：
```java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

@Component
// 业务类
public class AccountService {
    // 转账业务方法
    public void transfer(){
        System.out.println("正在进行银行账户转账");
    }
    // 取款业务方法
    public void withdraw(){
        System.out.println("正在进行取款操作");
    }
}

```
```java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

@Component
// 业务类
public class OrderService {
    // 生成订单
    public void generate(){
        System.out.println("正在生成订单");
    }
    // 取消订单
    public void cancel(){
        System.out.println("正在取消订单");
    }
}
```
注意，以上两个业务类已经纳入spring bean的管理，因为都添加了@Component注解。
接下来我们给以上两个业务类的4个方法添加事务控制代码，使用AOP来完成：
```java
package com.powernode.spring6.biz;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
// 事务切面类
public class TransactionAspect {
    
    @Around("execution(* com.powernode.spring6.biz..*(..))")
    public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint){
        try {
            System.out.println("开启事务");
            // 执行目标
            proceedingJoinPoint.proceed();
            System.out.println("提交事务");
        } catch (Throwable e) {
            System.out.println("回滚事务");
        }
    }
}

```
你看，这个事务控制代码是不是只需要写一次就行了，并且修改起来也没有成本。编写测试程序：
```java
package com.powernode.spring6.test;

import com.powernode.spring6.biz.AccountService;
import com.powernode.spring6.biz.OrderService;
import com.powernode.spring6.service.Spring6Configuration;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class AOPTest2 {
    @Test
    public void testTransaction(){
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Configuration.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
        AccountService accountService = applicationContext.getBean("accountService", AccountService.class);
        // 生成订单
        orderService.generate();
        // 取消订单
        orderService.cancel();
        // 转账
        accountService.transfer();
        // 取款
        accountService.withdraw();
    }
}
```
执行结果：  
![5F9597E7-7930-4384-95C2-CF64C9DDA9F3.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665899075177-6660d491-06d4-4296-b69a-b54716179c9d.png#averageHue=%23313131&clientId=u34f8a484-08bc-4&from=ui&height=365&id=ue7ff086e&originHeight=676&originWidth=846&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125447&status=done&style=shadow&taskId=u8dd026e7-67bb-486c-bc8b-8ce1fb7e15c&title=&width=457)

通过测试可以看到，所有的业务方法都添加了事务控制的代码。


### 2.AOP的实际案例：安全日志
需求是这样的：项目开发结束了，已经上线了。运行正常。客户提出了新的需求：凡**是在系统中进行修改操作的，删除操作的，新增操作的，都要把这个人记录下来**。因为这几个操作是属于危险行为。例如有业务类和业务方法：
```java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

@Component
//用户业务
public class UserService {
    public void getUser(){
        System.out.println("获取用户信息");
    }
    public void saveUser(){
        System.out.println("保存用户");
    }
    public void deleteUser(){
        System.out.println("删除用户");
    }
    public void modifyUser(){
        System.out.println("修改用户");
    }
}
```

```java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

// 商品业务类
@Component
public class ProductService {
    public void getProduct(){
        System.out.println("获取商品信息");
    }
    public void saveProduct(){
        System.out.println("保存商品");
    }
    public void deleteProduct(){
        System.out.println("删除商品");
    }
    public void modifyProduct(){
        System.out.println("修改商品");
    }
}


```
注意：已经添加了@Component注解。

接下来我们使用aop来解决上面的需求：编写一个负责安全的切面类
```java
package com.powernode.spring6.biz;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class SecurityAspect {

    @Pointcut("execution(* com.powernode.spring6.biz..save*(..))")
    public void savePointcut(){}

    @Pointcut("execution(* com.powernode.spring6.biz..delete*(..))")
    public void deletePointcut(){}

    @Pointcut("execution(* com.powernode.spring6.biz..modify*(..))")
    public void modifyPointcut(){}

    @Before("savePointcut() || deletePointcut() || modifyPointcut()")
    public void beforeAdivce(JoinPoint joinpoint){
        System.out.println("XXX操作员正在操作"+joinpoint.getSignature().getName()+"方法");
    }
}
```
* **注意这里使用\||来并多个切点表达式。**
```java
@Test
public void testSecurity(){
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Configuration.class);
    UserService userService = applicationContext.getBean("userService", UserService.class);
    ProductService productService = applicationContext.getBean("productService", ProductService.class);
    userService.getUser();
    userService.saveUser();
    userService.deleteUser();
    userService.modifyUser();
    productService.getProduct();
    productService.saveProduct();
    productService.deleteProduct();
    productService.modifyProduct();
}
```
执行结果：  
![5F9597E7-7930-4384-95C2-CF64C9DDA9F3.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1665901327786-9bfab382-61a3-4d1e-abe5-728b242eb3a2.png#averageHue=%23333333&clientId=u34f8a484-08bc-4&from=ui&height=425&id=ue0b3f754&originHeight=852&originWidth=844&originalType=binary&ratio=1&rotation=0&showTitle=false&size=203543&status=done&style=shadow&taskId=ucddfd42f-50b8-425d-9ace-c36b67eef63&title=&width=421)
