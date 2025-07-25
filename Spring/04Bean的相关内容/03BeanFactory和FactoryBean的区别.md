### 7.5.1 BeanFactory
Spring IoC容器的顶级对象，BeanFactory被翻译为“Bean工厂”，在Spring的IoC容器中，**Bean工厂”负责创建Bean对象。
BeanFactory是工厂**。

注：ApplicationContext继承自Beanfactory，所以他拥有创建Bean对象的功能
### 7.5.2 FactoryBean
FactoryBean：**它是一个Bean，是一个能够**辅助Spring**实例化其它Bean对象的一个Bean**。
在Spring中，Bean可以分为两类：
- 第一类：普通Bean
- 第二类：工厂Bean（记住：工厂Bean也是一种Bean，只不过这种Bean比较特殊，它可以辅助Spring实例化其它Bean对象。凡是实现了factoryBean接口的，都可以称之为工厂Bean ）