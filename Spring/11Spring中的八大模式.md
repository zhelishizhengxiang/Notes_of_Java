
## 19.1 简单工厂模式
BeanFactory的getBean()方法，通过唯一标识来获取Bean对象。是典型的简单工厂模式（静态工厂模式）；
## 19.2 工厂方法模式
FactoryBean是典型的工厂方法模式。在配置文件中通过factory-method属性来指定工厂方法，该方法是一个实例方法。
## 19.3 单例模式
Spring用的是双重判断加锁的单例模式。请看下面代码，我们之前讲解Bean的循环依赖的时候见过：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1666663352271-4ba8d737-1e32-4f0e-b01a-aa305ad3abea.png#averageHue=%23fcfbfa&clientId=u73ffb7ec-cc40-4&from=paste&height=839&id=u96f97bc2&originHeight=839&originWidth=1190&originalType=binary&ratio=1&rotation=0&showTitle=false&size=137222&status=done&style=shadow&taskId=u92873aaa-765a-4341-8094-6fa96146e4a&title=&width=1190)


## 19.4 代理模式
Spring的AOP就是使用了动态代理实现的。
## 19.5 装饰器模式
JavaSE中的IO流是非常典型的装饰器模式。
Spring 中配置 DataSource 的时候，这些dataSource可能是各种不同类型的，比如不同的数据库：Oracle、SQL Server、MySQL等，也可能是不同的数据源：比如apache 提供的org.apache.commons.dbcp.BasicDataSource、spring提供的org.springframework.jndi.JndiObjectFactoryBean等。
这时，能否在尽可能少修改原有类代码下的情况下，做到动态切换不同的数据源？此时就可以用到装饰者模式。
Spring根据每次请求的不同，将dataSource属性设置成不同的数据源，以到达切换数据源的目的。
**Spring中类名中带有：Decorator和Wrapper单词的类，都是装饰器模式。**


## 19.6 观察者模式
定义对象间的一对多的关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并自动更新。Spring中观察者模式一般用在listener的实现。
Spring中的事件编程模型就是观察者模式的实现。在Spring中定义了一个ApplicationListener接口，用来监听Application的事件，Application其实就是ApplicationContext，ApplicationContext内置了几个事件，其中比较容易理解的是：ContextRefreshedEvent、ContextStartedEvent、ContextStoppedEvent、ContextClosedEvent
## 19.7 策略模式
策略模式是行为性模式，调用不同的方法，适应行为的变化 ，强调父类的调用子类的特性 。
getHandler是HandlerMapping接口中的唯一方法，用于根据请求找到匹配的处理器。
比如我们自己写了AccountDao接口，然后这个接口下有不同的实现类：AccountDaoForMySQL，AccountDaoForOracle。对于service来说不需要关心底层具体的实现，只需要面向AccountDao接口调用，底层可以灵活切换实现，这就是策略模式。
## 19.8 模板方法模式
Spring中的JdbcTemplate类就是一个模板类。它就是一个模板方法设计模式的体现。在模板类的模板方法execute中编写核心算法，具体的实现步骤在子类中完成。
