
**在SpringMVC中，在request域中共享数据有以下几种方式**：  
1. **使用原生Servlet API方式：控制器方法上添加 HttpServletRequest参数即可**。
2. **使用Model接口：控制器方法的参数上添加一个接口类型：Model model(使用model来模拟request域)**。
3. **使用Map接口：控制器方法的参数上添加一个接口类型：Map map**。
4. **使用ModelMap类：控制器方法的参数上添加一个类型：ModelMap modelMap**。
5. **使用ModelAndView类：使用Spring MVC框架提供的ModelAndView类完成request域数据共享**。


## 1.使用原生Servlet API方式（不建议）
在Controller的方法上使用HttpServletRequest：
```java
package com.powernode.springmvc.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: RequestScopeTestController
 * Description:
 * Datetime: 2024/3/18 15:20
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class RequestScopeTestController {

    @RequestMapping("/testServletAPI")
    public String testServletAPI(HttpServletRequest request){
        // 向request域中存储数据
        request.setAttribute("testRequestScope", "在SpringMVC中使用原生Servlet API实现request域数据共享");
        //这个跳转，默认情况下是：转发的方式。（转发forward是一次请求）
        return "view";
    }
}

```


页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>view</title>
</head>
<body>
<div th:text="${testRequestScope}"></div>
</body>
</html>
```

超链接：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
<h1>Index Page</h1>
<a th:href="@{/testServletAPI}">在SpringMVC中使用原生Servlet API实现request域数据共享</a><br>
</body>
</html>
```
测试结果：      
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710747192867-9c718af0-41ad-4be9-8d48-c2ecdbd90789.png#averageHue=%23f9f7f5&clientId=u8e7ee681-52f6-4&from=paste&height=139&id=u50ed1ef3&originHeight=139&originWidth=489&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10113&status=done&style=shadow&taskId=uf9c8d6c8-09d7-4de3-87f2-9d60bbb31e9&title=&width=489)

这种方式当然可以，用SpringMVC框架，不建议使用原生Servlet API。


## 2.使用Model接口
```java
@RequestMapping("/testModel")
public String testModel(Model model){
    // 向request域中存储数据
    model.addAttribute("testRequestScope", "在SpringMVC中使用Model接口实现request域数据共享");
    return "view";
}
```


## 3.使用Map接口
```java
@RequestMapping("/testMap")
public String testMap(Map<String, Object> map){
    // 向request域中存储数据
    map.put("testRequestScope", "在SpringMVC中使用Map接口实现request域数据共享");
    return "view";
}
```
## 4.使用ModelMap类
```java
@RequestMapping("/testModelMap")
public String testModelMap(ModelMap modelMap){
    // 向request域中存储数据
    modelMap.addAttribute("testRequestScope", "在SpringMVC中使用ModelMap实现request域数据共享");
    return "view";
}
```

## Model、Map、ModelMap的关系
可以在以上Model、Map、ModelMap的测试程序中将其输出，看看输出什么：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710748328132-7ec71a48-8879-4758-824a-a9d669f1594a.png#averageHue=%23f7ece9&clientId=u8e7ee681-52f6-4&from=paste&height=384&id=u85bdc93d&originHeight=384&originWidth=845&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99039&status=done&style=shadow&taskId=u6e5eb772-76a5-4ac5-b577-dbd59d0b060&title=&width=845)
看不出来什么区别，从输出结果上可以看到都是一样的。  

可以将其运行时类名输出：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710748490407-0ab2044c-0261-498d-b55d-ce563afda27d.png#averageHue=%23f7ece8&clientId=u8e7ee681-52f6-4&from=paste&height=431&id=u0cb00398&originHeight=431&originWidth=788&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105090&status=done&style=shadow&taskId=ud7c08121-4be1-404b-8262-0a3d2870cfb&title=&width=788)
* 关系：**通过输出结果可以看出，无论是Model、Map还是ModelMap，底层实例化的对象都是：BindingAwareModelMap**。
* 查看BindingAwareModelMap的继承结构：  
	![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710748694354-caf9941e-9ce9-4215-bfe7-2d2a759ef206.png#averageHue=%23c3d8b1&clientId=u8e7ee681-52f6-4&from=paste&height=197&id=u6b7fdd17&originHeight=197&originWidth=511&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29224&status=done&style=shadow&taskId=u47f97067-c956-4144-845f-83754acefd8&title=&width=511)
	* **通过继承结构可以看出：BindingAwareModelMap继承了ModelMap，而ModelMap又实现了Map接口。**    
* 另外，请看以下源码：  
	![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710748884799-5bad9d0f-9926-4ef0-a29e-7f9e5d6bd383.png#averageHue=%23fdf8f3&clientId=u8e7ee681-52f6-4&from=paste&height=114&id=ubf08d967&originHeight=114&originWidth=757&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14104&status=done&style=shadow&taskId=uc4bbbf8a-7729-4dd8-8817-07b0db88bd0&title=&width=757)
	* **可以看出ModelMap又实现了Model接口。因此表面上是采用了不同方式，底层本质上是相同的。**

SpringMVC之所以提供了这些方式，目的就是方便程序员的使用，提供了多样化的方式，可见它的重要性。

## 5.使用ModelAndView类

* **在SpringMVC框架中为了更好的体现MVC架构模式，提供了一个类：ModelAndView。**
* **这个类的实例封装了Model和View。也就是说这个类既封装业务处理之后的数据，也体现了跳转到哪个视图。使用它也可以完成request域数据共享**。
```java
@RequestMapping("/testModelAndView")
public ModelAndView testModelAndView(){
    // 创建“模型与视图对象”
    ModelAndView modelAndView = new ModelAndView();
    // 绑定数据
    modelAndView.addObject("testRequestScope", "在SpringMVC中使用ModelAndView实现request域数据共享");
    // 绑定视图
    modelAndView.setViewName("view");
    // 返回
    return modelAndView;
}
```
这种方式需要注意的是：  
1. **方法的返回值类型不是String，而是ModelAndView对象**。
2. **ModelAndView不是出现在方法的参数位置，而是在方法体中new的**。
3. **需要调用addObject向域中存储数据**。
4. **需要调用setViewName设置视图的名字**。
5. **该方法的返回值其实是返回给了DispatcherServlet，由它来完成对该模型视图对象的处理工作**


## ModelAndView源码分析

以上我们通过了五种方式完成了request域数据共享，包括：原生Servlet API，Model、Map、ModelMap、ModelAndView

* **其中后四种：Model、Map、ModelMap、ModelAndView。这四种方式在底层DispatcherServlet调用我们的Controller之后，返回的对象都是ModelAndView，这个可以通过源码进行分析。**
* 当请求路径不是JSP的时候，都会走前端控制器DispatcherServlet。**DispatcherServlet中有一个核心方法 doDispatch()，这个方法用来通过请求路径找到对应的处理器(控制器)方法 。然后调用处理器方法，处理器方法返回一个逻辑视图名称（可能也会直接返回一个ModelAndView对象）,底层会将逻辑视图名称转换为View对象，然后将View对象结合Model对象，封装一个ModelAndView对象，然后将该对象返回给DispatcherServlet类了**

在以上四种方式中，拿Model举例，添加断点进行调试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710750710855-53e8ffdd-b563-453e-afb4-70648684e619.png#averageHue=%23fbf7f2&clientId=u8e7ee681-52f6-4&from=paste&height=257&id=u579b6322&originHeight=257&originWidth=749&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46267&status=done&style=shadow&taskId=u246cd96c-1cb9-4a9c-9cb4-f5878523f59&title=&width=749)

启动服务器，发送请求，走到断点：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710750795816-555dfc56-ccf2-43b4-b516-a737336d1e4f.png#averageHue=%23fbfaf8&clientId=u8e7ee681-52f6-4&from=paste&height=264&id=u38449060&originHeight=264&originWidth=591&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42900&status=done&style=shadow&taskId=u874dfc83-8c70-4887-9c8e-1a1945c0b59&title=&width=591)

查看VM Stack信息：        
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710750881676-99c9c130-a6d5-4751-8e71-6de12d3ba642.png#averageHue=%23fdf2d7&clientId=u8e7ee681-52f6-4&from=paste&height=664&id=uc7009af3&originHeight=664&originWidth=758&originalType=binary&ratio=1&rotation=0&showTitle=false&size=177558&status=done&style=shadow&taskId=u9e11f91d-5667-44ca-a156-dc9b1934355&title=&width=758)

查看DispatcherServlet的1089行，源码如下：      
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710750933440-8254f738-2716-4f56-8610-4814e6fdecbf.png#averageHue=%23fdf6f1&clientId=u8e7ee681-52f6-4&from=paste&height=201&id=u56c3d766&originHeight=201&originWidth=1123&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26567&status=done&style=shadow&taskId=u4b20400a-ba14-47de-a725-f6a2acf2f34&title=&width=1123)
* 该方法一执行就会调用控制器中的对应方法，并且返回对应的modelAndView对象。

可以看到这里，无论你使用哪种方式，最终都要返回一个ModelAndView对象。

提醒：大家可以通过以下断点调试方式，采用一级一级返回，最终可以看到都会返回ModelAndView对象。  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710751055879-078ad592-a894-45fe-8d4d-1a74d9c8db79.png#averageHue=%23b0d8c0&clientId=u8e7ee681-52f6-4&from=paste&height=71&id=u09e6abc5&originHeight=71&originWidth=357&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5747&status=done&style=shadow&taskId=u06fa03a6-0a46-4667-b39e-9453d475514&title=&width=357)

