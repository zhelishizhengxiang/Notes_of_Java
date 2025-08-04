# 一、学习本套教程前的知识储备

- **JavaSE**
- **HTML+CSS+JavaScript**
- **Vue**
- **AJAX + axios**
- **Thymeleaf**
- **Servlet**
- **Maven**
- **Spring**


# 二、什么是MVC
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


面试题：什么是三层模型，并说一说MVC架构模式与三层模型的区别？

三层模型：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711327157487-31faac36-0200-4dd9-afba-65842f7b7e30.png#averageHue=%23e9e9e8&clientId=u0db277a2-7f2a-4&from=paste&height=429&id=u5b1e0bf2&originHeight=740&originWidth=283&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10649&status=done&style=shadow&taskId=u0f2f5b57-991f-4ab1-aa3b-8614d0612aa&title=&width=164)                            ![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711327251994-1048ab41-5ecb-4f5d-b3ff-9bf9fd6feb0b.png#averageHue=%23f7f7f7&clientId=u0db277a2-7f2a-4&from=paste&height=426&id=ue3aac816&originHeight=792&originWidth=747&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17781&status=done&style=shadow&taskId=u771a4899-3c30-43cd-afee-3d7c990eccb&title=&width=402)

MVC 和三层模型都采用了分层结构来设计应用程序，都是降低耦合度，提高扩展力，提高组件复用性。区别在于：他们的关注点不同，三层模型更加关注业务逻辑组件的划分。
MVC架构模式关注的是整个应用程序的层次关系和分离思想。现代的开发方式大部分都是MVC架构模式结合三层模型一起用。


# 三、什么是SpringMVC
## SpringMVC概述
**SpringMVC是一个实现了MVC架构模式的Web框架，底层基于Servlet实现。**

**SpringMVC已经将MVC架构模式实现了，因此只要我们是基于SpringMVC框架写代码，编写的程序就是符合MVC架构模式的。**（MVC的架子搭好了，我们只需要添添补补）

Spring框架中有一个子项目叫做Spring Web，Spring Web子项目当中包含很多模块，例如：
- Spring MVC
- Spring WebFlux
- Spring Web Services
- Spring Web Flow
- Spring WebSocket
- Spring Web Services Client

可见 SpringMVC是Spring Web子项目当中的一个模块。**因此也可以说SpringMVC是Spring框架的一部分。**

所以学习SpringMVC框架之前要先学习Spring框架中的IoC和AOP等内容。
另外，使用SpringMVC框架的时候同样也可以使用IoC和AOP。

以下就是Spring官方给出的Spring架构图，其中Web中的servlet指的就是Spring MVC：  
![163550G63-0.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710215881989-618986f1-11c4-459a-8eaa-b58c4ee28d19.png#averageHue=%23404136&clientId=u4dcade31-9013-4&from=paste&height=475&id=u6664721a&originHeight=475&originWidth=647&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17253&status=done&style=shadow&taskId=u159efa0f-eecd-4b86-8737-63a6635acf0&title=&width=647)



## SpringMVC帮我们做了什么

SpringMVC框架帮我们做了什么，与纯粹的Servlet开发有什么区别？

1.  入口控制：**SpringMVC框架通过DispatcherServlet作为入口控制器，负责接收请求和分发请求。(即总Controller已经由SpringMVC框架提供，所有请求都先经过DispatcherServlet，由他来进将请求分发给其他Controller，)** 而在Servlet开发中，需要自己编写Servlet程序，并在web.xml中进行配置，才能接受和处理请求。 
2. **在SpringMVC中，表单提交时可以自动将表单数据绑定到相应的JavaBean对象中，只需要在控制器方法的参数列表中声明该JavaBean对象即可，无需手动获取和赋值表单数据**。而在纯粹的Servlet开发中，这些都是需要自己手动完成的。
3.  **IoC容器：SpringMVC框架通过IoC容器管理对象，只需要在配置文件中进行相应的配置即可获取实例对象**，而在Servlet开发中需要手动创建对象实例。 
4.  **统一处理请求：SpringMVC框架提供了拦截器、异常处理器等统一处理请求的机制，并且可以灵活地配置这些处理器**。而在Servlet开发中，需要自行编写过滤器、异常处理器等，增加了代码的复杂度和开发难度。 
5.  **视图解析：SpringMVC框架提供了多种视图模板，如Thymeleaf、JSP、Freemarker、Velocity等，并且支持国际化、主题等特性**。而在Servlet开发中需要手动处理视图层，增加了代码的复杂度。 

总之，与Servlet开发相比，SpringMVC框架可以帮我们节省很多时间和精力，减少代码的复杂度，更加专注于业务开发。同时，也提供了更多的功能和扩展性，可以更好地满足企业级应用的开发需求。

## SpringMVC框架的特点

1.  轻量级：相对于其他Web框架，Spring MVC框架比较小巧轻便。（只有几个几百KB左右的Jar包文件） 
2.  模块化：请求处理过程被分成多个模块，以模块化的方式进行处理。 
   3. 控制器模块：Controller
   4. 业务逻辑模块：Model
   5. 视图模块：View
6.  依赖注入：Spring MVC框架利用Spring框架的依赖注入功能实现对象的管理，实现松散耦合。 
7.  易于扩展：提供了很多口子，允许开发者根据需要插入自己的代码，以扩展实现应用程序的特殊需求。 
   8. Spring MVC框架允许开发人员通过自定义模块和组件来扩展和增强框架的功能。
   9. **Spring MVC框架与其他Spring框架及第三方框架集成得非常紧密，这使得开发人员可以非常方便地集成其他框架，以获得更好的功能。**
10.  **易于测试：支持单元测试框架，提高代码质量和可维护性。 （对SpringMVC中的Controller测试时，不需要依靠Web服务器。**）
11.  自动化配置：提供自动化配置，减少配置细节。 
   12. Spring MVC框架基于约定大于配置的原则，对常用的配置约定进行自动化配置。
13.  灵活性：Spring MVC框架支持多种视图技术，如JSP、FreeMarker、Thymeleaf、FreeMarker等，针对不同的视图配置不同的视图解析器即可。 


# 四、本套教程相关版本

- JDK版本：Java21
- Maven版本：3.9.6
- Tomcat版本：10
- Spring版本：6.1.4
- SpringMVC版本：6.1.4
- IDEA版本：2023.3
- Thymeleaf版本：3.1.2