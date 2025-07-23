![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=u48f9f116&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 学习本套教程前的知识储备

- **JavaSE**
- **HTML+CSS+JavaScript**
- **Vue**
- **AJAX + axios**
- **Thymeleaf**
- **Servlet**
- **Maven**
- **Spring**

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=jiGpo&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 什么是MVC
MVC架构模式相关课程，在老杜的JavaWeb课程中已经详细的讲解了，如果没有学过的，可以看这个视频：[https://www.bilibili.com/video/BV1Z3411C7NZ](https://www.bilibili.com/video/BV1Z3411C7NZ/?share_source=copy_web&vd_source=ec35128d1000684f9b28e503d6278a41)
MVC是一种软件架构模式（是一种软件架构设计思想，不止Java开发中用到，其它语言也需要用到），它将应用分为三块：

- M：Model（模型）
- V：View（视图）
- C：Controller（控制器）

应用为什么要被分为三块，优点是什么？

- 低耦合，扩展能力增强
- 代码复用性增强
- 代码可维护性增强
- 高内聚，让程序员更加专注业务的开发

MVC将应用分为三块，每一块各司其职，都有自己专注的事情要做，他们属于分工协作，互相配合：

- Model：负责业务处理及数据的收集。
- View：负责数据的展示
- Controller：负责调度。它是一个调度中心，它来决定什么时候调用Model来处理业务，什么时候调用View视图来展示数据。

MVC架构模式如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710142469881-5dee11e1-80e8-4cbc-8f0c-726d4e42bbfa.png#averageHue=%23fcfbfb&clientId=uce1673f1-7a5d-4&from=paste&height=490&id=u4dd05c22&originHeight=490&originWidth=1378&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59855&status=done&style=shadow&taskId=ua013f031-1935-4a3d-8005-8a873b6c68a&title=&width=1378)
MVC架构模式的描述：前端浏览器发送请求给web服务器，web服务器中的Controller接收到用户的请求，Controller负责将前端提交的数据进行封装，然后Controller调用Model来处理业务，当Model处理完业务后会返回处理之后的数据给Controller，Controller再调用View来完成数据的展示，最终将结果响应给浏览器，浏览器进行渲染展示页面。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=VmQoW&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

面试题：什么是三层模型，并说一说MVC架构模式与三层模型的区别？
三层模型：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711327157487-31faac36-0200-4dd9-afba-65842f7b7e30.png#averageHue=%23e9e9e8&clientId=u0db277a2-7f2a-4&from=paste&height=429&id=u5b1e0bf2&originHeight=740&originWidth=283&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10649&status=done&style=shadow&taskId=u0f2f5b57-991f-4ab1-aa3b-8614d0612aa&title=&width=164)                            ![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711327251994-1048ab41-5ecb-4f5d-b3ff-9bf9fd6feb0b.png#averageHue=%23f7f7f7&clientId=u0db277a2-7f2a-4&from=paste&height=426&id=ue3aac816&originHeight=792&originWidth=747&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17781&status=done&style=shadow&taskId=u771a4899-3c30-43cd-afee-3d7c990eccb&title=&width=402)

MVC 和三层模型都采用了分层结构来设计应用程序，都是降低耦合度，提高扩展力，提高组件复用性。区别在于：他们的关注点不同，三层模型更加关注业务逻辑组件的划分。
MVC架构模式关注的是整个应用程序的层次关系和分离思想。现代的开发方式大部分都是MVC架构模式结合三层模型一起用。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=kZ6Eh&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 什么是SpringMVC
## SpringMVC概述
SpringMVC是一个实现了MVC架构模式的Web框架，底层基于Servlet实现。
SpringMVC已经将MVC架构模式实现了，因此只要我们是基于SpringMVC框架写代码，编写的程序就是符合MVC架构模式的。（**MVC的架子搭好了，我们只需要添添补补**）
Spring框架中有一个子项目叫做Spring Web，Spring Web子项目当中包含很多模块，例如：

- Spring MVC
- Spring WebFlux
- Spring Web Services
- Spring Web Flow
- Spring WebSocket
- Spring Web Services Client

可见 SpringMVC是Spring Web子项目当中的一个模块。因此也可以说SpringMVC是Spring框架的一部分。
所以学习SpringMVC框架之前要先学习Spring框架中的IoC和AOP等内容。
另外，使用SpringMVC框架的时候同样也可以使用IoC和AOP。
以下就是Spring官方给出的Spring架构图，其中Web中的servlet指的就是Spring MVC：
![163550G63-0.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710215881989-618986f1-11c4-459a-8eaa-b58c4ee28d19.png#averageHue=%23404136&clientId=u4dcade31-9013-4&from=paste&height=475&id=u6664721a&originHeight=475&originWidth=647&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17253&status=done&style=shadow&taskId=u159efa0f-eecd-4b86-8737-63a6635acf0&title=&width=647)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=KLL0J&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

## SpringMVC帮我们做了什么
SpringMVC框架帮我们做了什么，与纯粹的Servlet开发有什么区别？

1.  入口控制：SpringMVC框架通过DispatcherServlet作为入口控制器，负责接收请求和分发请求。而在Servlet开发中，需要自己编写Servlet程序，并在web.xml中进行配置，才能接受和处理请求。 
2. 在SpringMVC中，表单提交时可以自动将表单数据绑定到相应的JavaBean对象中，只需要在控制器方法的参数列表中声明该JavaBean对象即可，无需手动获取和赋值表单数据。而在纯粹的Servlet开发中，这些都是需要自己手动完成的。
3.  IoC容器：SpringMVC框架通过IoC容器管理对象，只需要在配置文件中进行相应的配置即可获取实例对象，而在Servlet开发中需要手动创建对象实例。 
4.  统一处理请求：SpringMVC框架提供了拦截器、异常处理器等统一处理请求的机制，并且可以灵活地配置这些处理器。而在Servlet开发中，需要自行编写过滤器、异常处理器等，增加了代码的复杂度和开发难度。 
5.  视图解析：SpringMVC框架提供了多种视图模板，如JSP、Freemarker、Velocity等，并且支持国际化、主题等特性。而在Servlet开发中需要手动处理视图层，增加了代码的复杂度。 

总之，与Servlet开发相比，SpringMVC框架可以帮我们节省很多时间和精力，减少代码的复杂度，更加专注于业务开发。同时，也提供了更多的功能和扩展性，可以更好地满足企业级应用的开发需求。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=GI0Y0&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## SpringMVC框架的特点

1.  轻量级：相对于其他Web框架，Spring MVC框架比较小巧轻便。（只有几个几百KB左右的Jar包文件） 
2.  模块化：请求处理过程被分成多个模块，以模块化的方式进行处理。 
   1. 控制器模块：Controller
   2. 业务逻辑模块：Model
   3. 视图模块：View
3.  依赖注入：Spring MVC框架利用Spring框架的依赖注入功能实现对象的管理，实现松散耦合。 
4.  易于扩展：提供了很多口子，允许开发者根据需要插入自己的代码，以扩展实现应用程序的特殊需求。 
   1. Spring MVC框架允许开发人员通过自定义模块和组件来扩展和增强框架的功能。
   2. Spring MVC框架与其他Spring框架及第三方框架集成得非常紧密，这使得开发人员可以非常方便地集成其他框架，以获得更好的功能。
5.  易于测试：支持单元测试框架，提高代码质量和可维护性。 （对SpringMVC中的Controller测试时，不需要依靠Web服务器。）
6.  自动化配置：提供自动化配置，减少配置细节。 
   1. Spring MVC框架基于约定大于配置的原则，对常用的配置约定进行自动化配置。
7.  灵活性：Spring MVC框架支持多种视图技术，如JSP、FreeMarker、Thymeleaf、FreeMarker等，针对不同的视图配置不同的视图解析器即可。 

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=WDXEp&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 本套教程相关版本

- JDK版本：Java21
- Maven版本：3.9.6
- Tomcat版本：10
- Spring版本：6.1.4
- SpringMVC版本：6.1.4
- IDEA版本：2023.3
- Thymeleaf版本：3.1.2

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=JBVA4&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 第一个SpringMVC程序
## 创建Maven模块
第一步：创建Empty Project，起名：springmvc。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710230195763-9f5da726-de76-4ba3-9297-c912390a4df3.png#averageHue=%232b2e32&clientId=u4dcade31-9013-4&from=paste&height=691&id=u6e826b07&originHeight=691&originWidth=783&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53646&status=done&style=shadow&taskId=uf253c6e8-a4c3-457f-90b8-f12f0689297&title=&width=783)
第二步：设置springmvc工程的JDK版本：Java21。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710230237463-c2927a70-4e12-4ca5-9b5c-a554eb2524e2.png#averageHue=%232c2f33&clientId=u4dcade31-9013-4&from=paste&height=450&id=u2df3ec3b&originHeight=450&originWidth=1025&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49913&status=done&style=shadow&taskId=ube238351-cd5b-4e61-859e-5d65afdb2b5&title=&width=1025)
第三步：设置maven。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710230325830-67db9b0f-5f01-4804-86a4-5d7c0e72026b.png#averageHue=%232d3034&clientId=u4dcade31-9013-4&from=paste&height=726&id=u6eff3be0&originHeight=726&originWidth=982&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107845&status=done&style=shadow&taskId=uf51f6d8e-1dda-4a90-b09e-2a32042c0e8&title=&width=982)
第四步：创建Maven模块
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710230477117-f9659e1e-196e-4b8b-aa00-2a2a4b9e1a9b.png#averageHue=%232f343c&clientId=u4dcade31-9013-4&from=paste&height=170&id=ue8072ae5&originHeight=170&originWidth=354&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13198&status=done&style=shadow&taskId=u654baeb7-3032-4edc-a3c3-c20a8f6e13e&title=&width=354)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710234129822-b45b7ab8-0275-4828-b0f1-04bacdd4ee33.png#averageHue=%232c2e32&clientId=u2f9b3597-10b9-4&from=paste&height=819&id=ufe00ce61&originHeight=819&originWidth=918&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79340&status=done&style=shadow&taskId=ue99e2081-ff4d-432a-b9fc-b7d344c3356&title=&width=918)
第五步：将pom.xml文件中的打包方式修改为war
```xml
<groupId>com.powernode.springmvc</groupId>
<artifactId>springmvc-001</artifactId>
<version>1.0-SNAPSHOT</version>
<!-- 打包方式设置为war方式 -->
<packaging>war</packaging>
```
第六步：添加以下依赖
```xml
<dependencies>
    <!-- Spring MVC依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>6.1.4</version>
    </dependency>
    <!--日志框架Logback依赖-->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.5.3</version>
    </dependency>
    <!--Servlet依赖-->
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>6.0.0</version>
        <scope>provided</scope>
    </dependency>
    <!--Spring6和Thymeleaf整合依赖-->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring6</artifactId>
        <version>3.1.2.RELEASE</version>
    </dependency>
</dependencies>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=czxF4&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 添加web支持
第一步：在main目录下创建一个webapp目录
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710236651417-5d2d8216-0848-41b8-9dcd-82daf9928e60.png#averageHue=%232d3138&clientId=u28b24a89-f425-4&from=paste&height=331&id=u9088a24c&originHeight=331&originWidth=347&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23354&status=done&style=shadow&taskId=u22c28912-f26d-4b02-925f-a389b1d51d8&title=&width=347)
第二步：添加web.xml配置文件
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710236821253-9144e1b5-8e6a-441a-8aa7-8464a2f30bf4.png#averageHue=%232b2e31&clientId=u28b24a89-f425-4&from=paste&height=851&id=u6851adfa&originHeight=851&originWidth=1237&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73161&status=done&style=shadow&taskId=u80b56088-b844-4339-82e2-ebfee7927b6&title=&width=1237)
注意 web.xml 文件的位置：E:\Spring MVC\code\springmvc\springmvc-001\**src\main\webapp\WEB-INF\web.xml**
注意版本选择：6.0
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710236908015-b84441b8-72e4-41d4-bf50-aea212651f99.png#averageHue=%232c2f34&clientId=u28b24a89-f425-4&from=paste&height=326&id=u6a9dd62f&originHeight=326&originWidth=955&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32569&status=done&style=shadow&taskId=ub18a6482-e640-4074-a4a2-d48e08aa690&title=&width=955)
添加web支持后的目录结构：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710236989434-41f9727a-6944-40ee-899c-3aa58fa1904f.png#averageHue=%232e323a&clientId=u28b24a89-f425-4&from=paste&height=267&id=ub49f5f76&originHeight=267&originWidth=232&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14025&status=done&style=shadow&taskId=ud572ee2f-beae-4750-bf36-e33e0ee7891&title=&width=232)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=qyLy7&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 配置web.xml文件
Spring MVC是一个web框架，在javaweb中谁来负责接收请求，处理请求，以及响应呢？当然是Servlet。在SpringMVC框架中已经为我们写好了一个Servlet，它的名字叫做：DispatcherServlet，我们称其为前端控制器。既然是Servlet，那么它就需要在web.xml文件中进行配置：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
         version="6.0">

    <!--SpringMVC提供的前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!-- /* 表示任何一个请求都交给DispatcherServlet来处理 -->
        <!-- / 表示当请求不是xx.jsp的时候，DispatcherServlet来负责处理本次请求-->
        <!-- jsp本质就是Servlet，因此如果请求是jsp的话，应该走它自己的Servlet，而不应该走DispatcherServlet -->
        <!-- 因此我们的 url-pattern 使用 / -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

DispatcherServlet是SpringMVC框架为我们提供的最核心的类，它是整个SpringMVC框架的前端控制器，负责接收HTTP请求、将请求路由到处理程序、处理响应信息，最终将响应返回给客户端。DispatcherServlet是Web应用程序的主要入口点之一，它的职责包括：

1.  接收客户端的HTTP请求：DispatcherServlet监听来自Web浏览器的HTTP请求，然后根据请求的URL将请求数据解析为Request对象。 
2.  处理请求的URL：DispatcherServlet将请求的URL（Uniform Resource Locator）与处理程序进行匹配，确定要调用哪个控制器（Controller）来处理此请求。 
3.  调用相应的控制器：DispatcherServlet将请求发送给找到的控制器处理，控制器将执行业务逻辑，然后返回一个模型对象（Model）。 
4.  渲染视图：DispatcherServlet将调用视图引擎，将模型对象呈现为用户可以查看的HTML页面。 
5.  返回响应给客户端：DispatcherServlet将为用户生成的响应发送回浏览器，响应可以包括表单、JSON、XML、HTML以及其它类型的数据

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=h71oP&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 编写控制器FirstController
DispatcherServlet接收到请求之后，会根据请求路径分发到对应的Controller，Controller来负责处理请求的核心业务。在SpringMVC框架中Controller是一个普通的Java类（一个普通的POJO类，不需要继承任何类或实现任何接口），需要注意的是：POJO类要纳入IoC容器来管理，POJO类的生命周期由Spring来管理，因此要使用注解标注：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;

/**
 * ClassName: FirstController
 * Description:
 * Datetime: 2024/3/13 11:56
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class FirstController {
}

```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=jzufR&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 配置springmvc-servlet.xml文件
SpringMVC框架有它自己的配置文件，该配置文件的名字默认为：<servlet-name>-servlet.xml，默认存放的位置是WEB-INF 目录下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--组件扫描-->
    <context:component-scan base-package="com.powernode.springmvc.controller"/>
    <!--视图解析器-->
    <bean id="thymeleafViewResolver" class="org.thymeleaf.spring6.view.ThymeleafViewResolver">
        <!--作用于视图渲染的过程中，可以设置视图渲染后输出时采用的编码字符集-->
        <property name="characterEncoding" value="UTF-8"/>
        <!--如果配置多个视图解析器，它来决定优先使用哪个视图解析器，它的值越小优先级越高-->
        <property name="order" value="1"/>
        <!--当 ThymeleafViewResolver 渲染模板时，会使用该模板引擎来解析、编译和渲染模板-->
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring6.SpringTemplateEngine">
                <!--用于指定 Thymeleaf 模板引擎使用的模板解析器。模板解析器负责根据模板位置、模板资源名称、文件编码等信息，加载模板并对其进行解析-->
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring6.templateresolver.SpringResourceTemplateResolver">
                        <!--设置模板文件的位置（前缀）-->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!--设置模板文件后缀（后缀），Thymeleaf文件扩展名不一定是html，也可以是其他，例如txt，大部分都是html-->
                        <property name="suffix" value=".html"/>
                        <!--设置模板类型，例如：HTML,TEXT,JAVASCRIPT,CSS等-->
                        <property name="templateMode" value="HTML"/>
                        <!--用于模板文件在读取和解析过程中采用的编码字符集-->
                        <property name="characterEncoding" value="UTF-8"/>
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
</beans>
```
在WEB-INF目录下新建springmvc-servlet.xml文件，并且提供以上配置信息。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710309679329-4454ce41-d80a-42dc-abb7-683bd9397856.png#averageHue=%23f0f3f8&clientId=u0dd2e7db-835e-4&from=paste&height=265&id=ueec9abf4&originHeight=265&originWidth=296&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15499&status=done&style=shadow&taskId=u118f717f-100a-4863-83f9-800ca76eab0&title=&width=296)
以上配置主要两项：

- 第一项：组件扫描。spring扫描这个包中的类，将这个包中的类实例化并纳入IoC容器的管理。
- 第二项：视图解析器。视图解析器（View Resolver）的作用主要是将Controller方法返回的逻辑视图名称解析成实际的视图对象。视图解析器将解析出的视图对象返回给DispatcherServlet，并最终由DispatcherServlet将该视图对象转化为响应结果，呈现给用户。

注意：如果采用了其它视图，请配置对应的视图解析器，例如：

- JSP的视图解析器：InternalResourceViewResolver
- FreeMarker视图解析器：FreeMarkerViewResolver
- Velocity视图解析器：VelocityViewResolver

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=rY0Jq&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 提供视图
在WEB-INF目录下新建templates目录，在templates目录中新建html文件，例如：first.html，并提供以下代码：
```html
<!DOCTYPE html>
<!--指定 th 命名空间，让 Thymeleaf 标准表达式可以被解析和执行-->
<!--th不是固定的，可以指定其它的命名空间，只不过大部分情况下用th-->
<!--表示程序中出现的 th 开头的后面代码都是 Thymeleaf语法，需要被 Thymeleaf识别-->
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>first springmvc</title>
</head>
<body>
<h1>我的第一个Spring MVC程序</h1>
</body>
</html>
```
对于每一个Thymeleaf文件来说 xmlns:th="[http://www.thymeleaf.org"](http://www.thymeleaf.org") 是必须要写的，为了方便后续开发，可以将其添加到html模板文件中：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710310831388-377e7bc4-f5b2-4fa3-9410-d90bfdd894b8.png#averageHue=%2381baa0&clientId=u0dd2e7db-835e-4&from=paste&height=543&id=u0d2806df&originHeight=543&originWidth=1159&originalType=binary&ratio=1&rotation=0&showTitle=false&size=106487&status=done&style=shadow&taskId=u203bebbc-500e-4cac-af4e-a5ffd79afe9&title=&width=1159)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=L9Yg4&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 控制器FirstController处理请求返回逻辑视图名称
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: FirstController
 * Description:
 * Datetime: 2024/3/13 11:56
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class FirstController {
    @RequestMapping(value="/haha")
    public String 名字随意(){
        System.out.println("正在处理请求....");
        // 返回逻辑视图名称（决定跳转到哪个页面）
        return "first";
    }
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=JeSVo&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 测试
第一步：配置Tomcat服务器
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710311594124-3fb7be8b-9029-4b30-990c-78d2233fb404.png#averageHue=%23f3f5f8&clientId=u0dd2e7db-835e-4&from=paste&height=719&id=ue0c3bfa6&originHeight=719&originWidth=1152&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84996&status=done&style=shadow&taskId=uea40fb78-9d1c-43d0-957d-4f9344ed40c&title=&width=1152)
第二步：部署web模块到Tomcat服务器
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710311627612-75b1508f-10fe-4510-840f-1b856fee1932.png#averageHue=%23f4f5f9&clientId=u0dd2e7db-835e-4&from=paste&height=722&id=u88b9e5d0&originHeight=722&originWidth=1155&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48659&status=done&style=shadow&taskId=u0aa86c21-f71f-4805-9d99-57a70ef4c90&title=&width=1155)
第三步：启动Tomcat服务器。如果在控制台输出的信息有中文乱码，请修改tomcat服务器配置文件：apache-tomcat-10.1.19\conf\logging.properties
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710319742230-840a1af7-4ba7-4ee2-bc9c-0f1a056b222a.png#averageHue=%23edeae8&clientId=u09801095-675e-4&from=paste&height=795&id=ua5e8396a&originHeight=795&originWidth=1181&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97129&status=done&style=shadow&taskId=u39e641a0-e822-41a8-af2e-142dcc6b03c&title=&width=1181)
第四步：打开浏览器，在浏览器地址栏上输入地址：http://localhost:8080/springmvc/haha
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710312173540-4e975a51-c0df-47a3-8bdd-f2fbdb8ad831.png#averageHue=%23f0efee&clientId=u0dd2e7db-835e-4&from=paste&height=186&id=u8c054553&originHeight=186&originWidth=482&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14690&status=done&style=shadow&taskId=ub2863046-844b-4739-9104-23161cb4703&title=&width=482)
后端控制台输出：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710312475793-dfb94231-5efe-4a72-840f-63d72143d47f.png#averageHue=%23faf5f3&clientId=u0dd2e7db-835e-4&from=paste&height=60&id=ude4fd229&originHeight=60&originWidth=297&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5273&status=done&style=shadow&taskId=u0ab8cffa-e1db-4cba-b5b3-ab7d1827efe&title=&width=297)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=hMFB9&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 执行流程总结

1. 浏览器发送请求：http://localhost:8080/springmvc/haha
2. SpringMVC的前端控制器DispatcherServlet接收到请求
3. DispatcherServlet根据请求路径 /haha 映射到 FirstController#名字随意()，调用该方法
4. FirstController#名字随意() 处理请求
5. FirstController#名字随意() 返回逻辑视图名称 first 给视图解析器
6. 视图解析器找到 /WEB-INF/templates/first.html 文件，并进行解析，生成视图解析对象返回给前端控制器DispatcherServlet
7. 前端控制器DispatcherServlet响应结果到浏览器。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=nInVb&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

## 一个Controller可以编写多个方法
一个Controller可以提供多个方法，每个方法通常是处理对应的请求，例如：
```java
@Controller
public class FirstController {
    @RequestMapping(value="/haha")
    public String 名字随意(){
        System.out.println("正在处理请求....");
        // 返回逻辑视图名称（决定跳转到哪个页面）
        return "first";
    }
    
    @RequestMapping("/other")
    public String other(){
        System.out.println("正在处理其它请求...");
        return "other";
    }
}
```
提供 other.html 文件
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>other</title>
</head>
<body>
<h1>other ...</h1>
</body>
</html>
```
在 first.html 文件中，添加超链接，用超链接发送 /other 请求：
```html
<!DOCTYPE html>
<!--指定 th 命名空间，让 Thymeleaf 标准表达式可以被解析和执行-->
<!--th不是固定的，可以指定其它的命名空间，只不过大部分情况下用th-->
<!--表示程序中出现的 th 开头的后面代码都是 Thymeleaf语法，需要被 Thymeleaf识别-->
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>first springmvc</title>
</head>
<body>
<h1>我的第一个Spring MVC程序</h1>
<!-- th: 表示后面的代码可以编写Thymeleaf语法，可以被Thymeleaf语法解析 -->
<!-- Thymeleaf检测到以 / 开始，表示绝对路径，自动会将webapp的上下文路径加上去 -->
<!-- 最终的效果是：href="/springmvc/other" -->
<a th:href="@{/other}">other请求</a>
</body>
</html>
```
启动Tomcat，打开浏览器，输入请求路径：http://localhost:8080/springmvc/haha
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710313350020-eaecbfaf-c1ba-44d1-9422-3371248f69a2.png#averageHue=%23f0efee&clientId=u0dd2e7db-835e-4&from=paste&height=193&id=u5a81f52c&originHeight=193&originWidth=470&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15658&status=done&style=shadow&taskId=u4443d95d-723e-49d2-a72c-e1e6b8ed38f&title=&width=470)
点击超链接：other请求
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710313367394-acfcdb9e-576c-4cc2-8d08-2858854a947e.png#averageHue=%23f9f9f8&clientId=u0dd2e7db-835e-4&from=paste&height=160&id=u773a31a3&originHeight=160&originWidth=416&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7427&status=done&style=shadow&taskId=uc14056fa-bf74-46b1-812a-87722cb2245&title=&width=416)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=gbso3&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 第二个SpringMVC程序
## 创建Maven模块

1. pom.xml文件中添加依赖
   1. springmvc依赖
   2. logback依赖
   3. servlet依赖（scope为provided）
   4. thymeleaf与spring6整合依赖
2. 打包方式war

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=IPU2Z&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 添加web支持
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710315550290-18c819de-15fb-4653-a242-8ac1c8d1255d.png#averageHue=%23eef1f8&clientId=u0dd2e7db-835e-4&from=paste&height=198&id=u1bc1a1cd&originHeight=198&originWidth=219&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9851&status=done&style=shadow&taskId=u9ae36ed0-8799-403a-843d-6ea0c0f28a6&title=&width=219)
webapp目录没有小蓝点怎么办？添加web支持
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710315591600-e1e8f89d-9731-40ee-b601-04c8b2923258.png#averageHue=%23b2cec4&clientId=u0dd2e7db-835e-4&from=paste&height=373&id=u6dc66d24&originHeight=373&originWidth=369&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33616&status=done&style=shadow&taskId=u8345e6f1-5eb2-4184-bb3b-37bceb8ac17&title=&width=369)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710315690201-7d425088-0775-4e64-97a3-e33c09374add.png#averageHue=%23f4f5f9&clientId=u0dd2e7db-835e-4&from=paste&height=590&id=u16041e99&originHeight=590&originWidth=1359&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44597&status=done&style=shadow&taskId=u685dcb7d-f2e7-4a7b-a636-3807141b073&title=&width=1359)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=TdHdN&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 配置web.xml文件
重点：SpringMVC配置文件的名字和路径是可以手动设置的，如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--手动设置springmvc配置文件的路径及名字-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--为了提高用户的第一次访问效率，建议在web服务器启动时初始化前端控制器-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```
**通过<init-param>来设置SpringMVC配置文件的路径和名字。在DispatcherServlet的init方法执行时设置的。**
**<load-on-startup>1</load-on-startup>建议加上，这样可以提高用户第一次访问的效率。表示在web服务器启动时初始化DispatcherServlet。**

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=XEPhx&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 编写IndexController
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: IndexController
 * Description:
 * Datetime: 2024/3/13 15:47
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class IndexController {
    @RequestMapping("/")
    public String toIndex(){
        return "index";
    }
}
```
表示请求路径如果是：[http://localhost:8080/springmvc/](http://localhost:8080/springmvc/) ，则进入 /WEB-INF/templates/index.html 页面。
**这就是项目的首页效果！！！！！**

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=TG1Ma&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 在resources目录下配置springmvc.xml文件
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710316235461-25d719f5-5b8f-4600-922a-8568d9cd63dc.png#averageHue=%23f0f3f8&clientId=u0dd2e7db-835e-4&from=paste&height=315&id=u6e0a9fe3&originHeight=315&originWidth=366&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21866&status=done&style=shadow&taskId=ubf2c7ec6-0ca1-437f-9ab5-941d3f16c50&title=&width=366)
配置内容和之前一样，一个是视图解析器，一个是组件扫描。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Y3hK7&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 提供视图
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710316353838-aac1cd57-12e3-47e4-8b73-2ea2a07a0954.png#averageHue=%23eef0f4&clientId=u0dd2e7db-835e-4&from=paste&height=128&id=u9a159361&originHeight=128&originWidth=289&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7935&status=done&style=shadow&taskId=uaf3f4b6a-fb23-4ec5-8294-00978351f60&title=&width=289)
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>index page</title>
</head>
<body>
<h1>index page</h1>
</body>
</html>
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=QSrlq&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 测试
部署到web服务器，启动web服务器，打开浏览器，在地址栏上输入：[http://localhost:8080/springmvc/](http://localhost:8080/springmvc/)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710317491301-4104920d-3537-40d1-b950-2ad1f3398a2d.png#averageHue=%23f7f7f6&clientId=u0dd2e7db-835e-4&from=paste&height=170&id=uc8b7b484&originHeight=170&originWidth=421&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8615&status=done&style=shadow&taskId=u9896bf63-92fb-4dd5-9584-0cbffeb8277&title=&width=421)
