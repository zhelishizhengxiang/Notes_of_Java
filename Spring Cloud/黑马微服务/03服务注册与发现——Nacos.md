在上一章我们实现了微服务拆分，并且通过Http请求实现了跨微服务的远程调用。不过这种手动发送Http请求的方式存在一些问题。  
试想一下，假如商品微服务被调用较多，为了应对更高的并发，我们进行了多实例部署，如图：    
![[file-20260323143402658.png]]

此时，每个`item-service`的实例其IP或端口不同，问题来了：   
- item-service这么多实例，cart-service如何知道每一个实例的地址？
- http请求要写url地址，`cart-service`服务到底该调用哪个实例呢？
- 如果在运行过程中，某一个`item-service`实例宕机，`cart-service`依然在调用该怎么办？
- 如果并发太高，`item-service`临时多部署了N台实例，`cart-service`如何知道新实例的地址？
    
为了解决上述问题，就必须引入注册中心的概念了，接下来我们就一起来分析下注册中心的原理。

### 1.注册中心原理
在微服务远程调用的过程中，包括两个角色：  
- 服务提供者：提供接口供其它微服务访问，比如`item-service`
- 服务消费者：调用其它微服务提供的接口，比如`cart-service`
    
在大型微服务项目中，服务提供者的数量会非常多，为了管理这些服务就引入了**注册中心**的概念。注册中心、服务提供者、服务消费者三者间关系如下：    
![[file-20260323224809419.png]]

流程如下：
1. **服务启动时就会注册自己的服务信息（服务名、IP、端口）到注册中心
2. **调用者可以从注册中心订阅想要的服务，获取服务对应的实例列表（1个服务可能多实例部署）
3. **调用者自己对实例列表负载均衡，挑选一个实例
4. **调用者向该实例发起远程调用
    

  当服务提供者的实例宕机或者启动新实例时，调用者如何得知呢？
- **服务提供者会定期向注册中心发送请求，报告自己的健康状态（心跳请求）**
- **当注册中心长时间收不到提供者的心跳时，会认为该实例宕机，将其从服务的实例列表中剔除
- **当服务有新实例启动时，会发送注册服务请求，其信息会被记录在注册中心的服务实例列表
- **当注册中心服务列表变更时，会主动通知微服务，更新本地服务列表

### 2.Nacos注册中心

目前开源的注册中心框架有很多，国内比较常见的有：
- Eureka：Netflix公司出品，目前被集成在SpringCloud当中，一般用于Java应用
- **Nacos：Alibaba公司出品，目前被集成在SpringCloudAlibaba中，一般用于Java应用**
- Consul：HashiCorp公司出品，目前集成在SpringCloud中，不限制微服务语言
    

以上几种注册中心都遵循SpringCloud中的API规范，因此在业务开发使用上没有太大差异。**由于Nacos是国内产品，中文文档比较丰富，而且同时具备配置管理功能（后面会学习），因此在国内使用较多**，课堂中我们会Nacos为例来学习。

官方网站：[Nacos官网| Nacos 配置中心 | Nacos 下载| Nacos 官方社区 | Nacos 官网](https://nacos.io/)

前言：使用docker nacos与windows nacos的配置的不同之处具体如下：  
![[file-20260408151835431.png]]
![[file-20260408151920191.png]] 

**我们基于Docker来部署Nacos的注册中心，首先我们要准备MySQL数据库表，用来存储Nacos的数据**。由于是Docker部署，所以大家需要将资料中的SQL文件导入到你**Docker中的MySQL容器**中：   
![[file-20260323224837894.png]]

最终表结构如下：    
![[file-20260323224846012.png]]

然后，找到课前资料下的nacos文件夹：   
![[file-20260323224854049.png]]

其中的`nacos/custom.env`文件中，有一个MYSQL_SERVICE_HOST也就是mysql地址，需要修改为你自己的虚拟机IP地址：   
![[file-20260323224904188.png]]

然后，将课前资料中的`nacos`目录上传至虚拟机的`/root`目录。  

进入root目录，然后执行下面的docker命令，即可完成nacos镜像的拉取以及容器的创建于启动。
```PowerShell
docker run -d \
--name nacos \
--env-file ./nacos/custom.env \
-p 8848:8848 \
-p 9848:9848 \
-p 9849:9849 \
--restart=always \
nacos/nacos-server:v2.1.0-slim
```

==**注：nacos服务端访问地址的端口为：ip地址:8848/nacos**==

启动完成后，访问下面地址：[http://192.168.150.101:8848/nacos/](http://192.168.150.101:8848/nacos/)，注意将`192.168.150.101`替换为你自己的虚拟机IP地址。

首次访问会跳转到登录页，**账号密码都是nacos**     


### 3.服务注册（将本服务信息注册到nacos）
接下来，我们把`item-service`注册到Nacos，步骤如下：  
1. **引入依赖**
2. **配置Nacos地址**
3. 重启

#### 3.1添加依赖

在`item-service`的`pom.xml`中添加依赖：
```XML
<!--nacos 服务注册发现-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

#### 3.2配置Nacos

在`item-service`的`application.yml`中添加nacos地址配置：  
```YAML
#配置服务名称
spring:
  application:
    name: item-service # 服务名称4
  # 配置nacos的地址坐服务注册
  cloud:
    nacos:
      server-addr: 192.168.150.101:8848 # nacos地址
```

  

#### 3.3启动服务实例
为了测试一个服务多个实例的情况，我们再配置一个`item-service`的部署实例：   
![[file-20260323225100802.png]]

然后配置启动项，注意重命名并且配置新的端口，避免冲突：   
![[file-20260323225115110.png]]

重启`item-service`的两个实例：  
![[file-20260323225123738.png]]

  
访问nacos控制台，可以发现服务注册成功：   
![[file-20260323225131033.png]]

点击详情，可以查看到`item-service`服务的两个实例信息：     
![[file-20260323225139680.png]]

### 4.服务发现（从注册中心区获取列表）

**服务的消费者要去nacos拉取服务，这个过程就是服务发现**，步骤如下：
- 引入依赖
- 配置Nacos地址
- 发现并调用服务
#### 4.1引入依赖

我们在`cart-service`中的`pom.xml`中添加下面的依赖： 
```XML
<!--nacos 服务注册发现-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

可以发现，这里Nacos的依赖于服务注册时一致，这个依赖中同时包含了服务注册和发现的功能。因为任何一个微服务都可以调用别人，也可以被别人调用，即可以是调用者，也可以是提供者。

因此，等一会儿`cart-service`启动，同样会注册到Nacos  
#### 4.2配置Nacos地址

在`cart-service`的`application.yml`中添加nacos地址配置：  
```YAML
spring:
  cloud:
    nacos:
      server-addr: 192.168.150.101:8848
```


#### 4.3发现并调用服务
接下来，服务调用者`cart-service`就可以去订阅`item-service`服务了。不过item-service有多个实例，而真正发起调用时只需要知道一个实例的地址。

因此，服务调用者必须利用负载均衡的算法，从多个实例中挑选一个去访问。常见的负载均衡算法有：
- 随机
- 轮询
- IP的hash
- 最近最少访问
- ...
这里我们可以选择最简单的随机负载均衡。


另外，服务发现需要用到一个工具，DiscoveryClient，SpringCloud已经帮我们自动装配，我们可以直接注入使用：  
![[file-20260323225223417.png]]

接下来，我们就可以对原来的远程调用做修改了，之前调用时我们需要写死服务提供者的IP和端口：    
![[file-20260323225231578.png]]

但现在不需要了，我们通过DiscoveryClient发现服务实例列表，然后通过负载均衡算法，选择一个实例去调用：    
![[file-20260323225240508.png]]
经过swagger测试，发现没有任何问题。  

### 5.配置中心
微服务开发时经常会遇到下面的这类问题：
- 网关路由在配置文件中写死了，如果变更必须重启微服务
- 某些业务配置在配置文件中写死了，每次修改都要重启服务
- 每个微服务都有很多重复的配置，维护成本高

这些问题都可以通过统一的**配置管理器服务**解决。而**Nacos不仅仅具备注册中心功能，也具备配置管理的功能**：   
![[file-20260419015037611.png]]

**微服务共享的配置可以统一交给Nacos保存和管理，在Nacos控制台修改配置后，Nacos会将配置变更推送给相关的微服务，并且无需重启即可生效，实现配置热更新**。

**网关的路由同样是配置，因此同样可以基于这个功能实现动态路由功能，无需重启网关即可修改路由配置**。

#### 5.1.配置共享

我们可以把微服务共享的配置抽取到Nacos中统一管理，这样就不需要每个微服务都重复配置了。分为两步：

- 在Nacos中添加共享配置
- 微服务拉取配置
##### 5.1.1.添加共享配置

以cart-service为例，我们看看有哪些配置是重复的，可以抽取的：

首先是jdbc相关配置：     
![[file-20260419015050895.png]]

然后是日志配置：  
![[file-20260419015114743.png]]

然后是swagger以及OpenFeign的配置：   
![[file-20260419015131806.png]]

我们在nacos控制台分别添加这些配置。

首先是jdbc相关配置，在`配置管理`->`配置列表`中点击`+`新建一个配置：   
![[file-20260419022126232.png]]

在弹出的表单中填写信息：   
![[file-20260419022136179.png]]

其中详细的配置如下：  
```YAML
spring:
  datasource:
    url: jdbc:mysql://${hm.db.host:192.168.150.101}:${hm.db.port:3306}/${hm.db.database}?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: ${hm.db.un:root}
    password: ${hm.db.pw:123}
mybatis-plus:
  configuration:
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
  global-config:
    db-config:
      update-strategy: not_null
      id-type: auto
```

注意这里的jdbc的相关参数并没有写死，例如：
- `数据库ip`：通过`${hm.db.host:192.168.150.101}`配置了默认值为`192.168.150.101`，**同时允许通过`${hm.db.host}`来覆盖默认值**
- `数据库端口`：通过`${hm.db.port:3306}`配置了默认值为`3306`，**同时允许通过`${hm.db.port}`来覆盖默认值**
- **`数据库database`：可以通过`${hm.db.database}`来设定，无默认值**
  
然后是统一的日志配置，命名为`shared-log.``yaml`，配置内容如下：
```YAML
logging:
  level:
    com.hmall: debug
  pattern:
    dateformat: HH:mm:ss:SSS
  file:
    path: "logs/${spring.application.name}"
```
  
然后是统一的swagger配置，命名为`shared-swagger.yaml`，配置内容如下：
```YAML
knife4j:
  enable: true
  openapi:
    title: ${hm.swagger.title:黑马商城接口文档}
    description: ${hm.swagger.description:黑马商城接口文档}
    email: ${hm.swagger.email:zhanghuyi@itcast.cn}
    concat: ${hm.swagger.concat:虎哥}
    url: https://www.itcast.cn
    version: v1.0.0
    group:
      default:
        group-name: default
        api-rule: package
        api-rule-resources:
          - ${hm.swagger.package}
```
注意，这里的swagger相关配置我们没有写死，例如：
- `title`：接口文档标题，我们用了`${hm.swagger.title}`来代替，将来可以有用户手动指定
- `email`：联系人邮箱，我们用了`${hm.swagger.email:``zhanghuyi@itcast.cn``}`，默认值是`zhanghuyi@itcast.cn`，同时允许用户利用`${hm.swagger.email}`来覆盖。
- `description`：描述使用`${hm.swagger.description:黑马商城接口文档}`
- `package`：使用`${hm.swagger.package}`

##### 5.1.2.拉取共享配置

接下来，我们要在微服务拉取共享配置。将拉取到的共享配置与本地的`application.yaml`配置合并，完成项目上下文的初始化。

**springCloud项目与传统springboot项目不同，它会有自己的一套上下文ApplicationContext，在启动的时候先去拉取Nacos中的配置来初始化spring cloud的上下文，ApplicationContext，之后才去完成加载springboot的配置文件，初始化springboot的上下文。**   
![[file-20260419021242294.png]]

不过，需要**注意的是，读取Nacos配置是SpringCloud上下文（`ApplicationContext`）初始化时处理的，发生在项目的引导阶段。然后才会初始化SpringBoot上下文，去读取`application.yaml`**。

也就是说，**由于nacos的地址是配置在applicaiton.yml中的，那么在引导阶段，`application.yaml`文件尚未读取，根本不知道nacos地址，所以根本无法成功拉取配置**，

**为了解决这个问题，SpringCloud在初始化上下文的时候会先读取一个名为`bootstrap.yaml`(或者`bootstrap.properties`)的文件，如果我们将nacos地址配置到`bootstrap.yaml`中，那么在项目引导阶段就可以读取nacos中的配置了**。       
![[file-20260419022206875.png]]

> **注：bootstrap文件和application文件一样，同样可以分成dev、local等环境来引入不同环境的配置。此处这个还可以交叉使用**，比如说在boostrap中定义`spring.profile.active: dev`，此处不仅会引入bootstrap-dev的配置文件，还会引入applicaiton-dev的配置文件信息。具体如下：   
![[file-20260419023945500.png]]
![[file-20260419023956041.png]]
![[file-20260419024012467.png]]

>注：**程配置中心的配置** 优先级比 **本地** `**application.yml**` **更高**，可以通过 `spring.cloud.config.override-none=true` 调整     
![[file-20260419154559715.png]]


因此，微服务整合Nacos配置管理的步骤如下：

###### 1）引入依赖：

在cart-service模块引入依赖：  
```XML
  <!--nacos配置管理-->
  <dependency>
      <groupId>com.alibaba.cloud</groupId>
      <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
  </dependency>
  <!--读取bootstrap文件-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-bootstrap</artifactId>
  </dependency>
```

###### 2）新建bootstrap.yaml

在cart-service中的resources目录新建一个bootstrap.yaml文件：    
![[file-20260419022218399.png]]

内容如下：
```YAML
spring:
  application:
    name: cart-service # 服务名称
  profiles:
    active: dev
  cloud:
    nacos:
      server-addr: 192.168.150.101 # nacos地址
      config:
        file-extension: yaml # 文件后缀名
        shared-configs: # 共享配置
          - dataId: shared-jdbc.yaml # 共享mybatis配置
          - dataId: shared-log.yaml # 共享日志配置
          - dataId: shared-swagger.yaml # 共享日志配置
```
###### 3）修改application.yaml

由于一些配置挪到了bootstrap.yaml，因此application.yaml需要修改为：

```YAML
server:
  port: 8082
feign:
  okhttp:
    enabled: true # 开启OKHttp连接池支持
hm:
  swagger:
    title: 购物车服务接口文档
    package: com.hmall.cart.controller
  db:
    database: hm-cart
```

重启服务，发现所有配置都生效了。


#### 5.2.配置热更新

有很多的业务相关参数，将来可能会根据实际情况临时调整。例如购物车业务，购物车数量有一个上限，默认是10，对应代码如下：   
![[file-20260419025109549.png]]

现在这里购物车是写死的固定值，我们应该将其配置在配置文件中，方便后期修改。

但现在的问题是，即便写在配置文件中，修改了配置还是需要重新打包、重启服务才能生效。能不能不用重启，直接生效呢？

这就要用到**Nacos的配置热更新：当修改配置文件中的配置时，微服务无需重启配置即可生效**，分为两步：
- **在Nacos中添加配置**
- **在微服务读取配置**

总结就是如下图所示：    
![[file-20260419160809894.png]]
* **第二步更推荐使用方式一**
*  **只要是在bootstrap配置了`spring.application.name`、`spring.active.profile`和`file-extension`，将来项目一启动，就会自动去配置中心找该文件读取配置，无需像共享配置那样再声明该配置文件的名称**    
	![[file-20260419162452532.png]]
	![[file-20260419162747133.png]]
* **设置配置文件名称时的后缀名可选，若不加后缀名，则不管是dev还是local环境都可以加载该配置**
##### 5.2.1.添加配置到Nacos

首先，我们在nacos中添加一个配置文件，将购物车的上限数量添加到配置中：  
![[file-20260419025136093.png]]

1. **注意文件的dataId格式：必须是与微服务名称有关**：（因为需要配置配置热更新的配置项，往往是与业务有关的配置项，肯定与某个和微服务有关）
```Plain
[服务名]-[spring.active.profile].[后缀名]
```
![[file-20260419160321894.png]]
2. 文件名称由三部分组成：
	- **`服务名`：我们是购物车服务**，所以是`cart-service`
	- **`spring.active.profile`：就是spring boot中的`spring.active.profile`，可以省略，则所有profile共享该配置**
	- **`后缀名`**：例如yaml


这里我们直接使用`cart-service.yaml`这个名称，则不管是dev还是local环境都可以共享该配置。

3. 配置内容如下：
```YAML
hm:
  cart:
    maxAmount: 1 # 购物车商品数量上限
```

  
提交配置，在控制台能看到新添加的配置：  
![[file-20260419025149816.png]]


##### 5.2.2.配置热更新

接着，我们在微服务中读取配置，实现配置热更新。

在`cart-service`中新建一个属性读取类：   
![[file-20260419025159191.png]]

代码如下：
```Java
package com.hmall.cart.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Data
@Component
@ConfigurationProperties(prefix = "hm.cart")
public class CartProperties {
    private Integer maxAmount;
}
```


接着，在业务中使用该属性加载类：   
![[file-20260419025209155.png]]


测试，向购物车中添加多个商品：  
![[file-20260419025216902.png]]

我们在nacos控制台，将购物车上限配置为5：  
![[file-20260419025234286.png]]
 

无需重启，再次测试购物车功能：   
![[file-20260419025249671.png]]

加入成功！无需重启服务，配置热更新就生效了！


### 6.Nacos补充内容（命名空间）

  上面讲的都是nacos最基本的内容，更多更重要的内容请参考[尚硅谷2025最新SpringCloud速通-操作步骤（详细）_尚硅谷2025最新springcloud速通-操作步骤(详细)-CSDN博客](https://blog.csdn.net/weixin_56884174/article/details/145573890)

