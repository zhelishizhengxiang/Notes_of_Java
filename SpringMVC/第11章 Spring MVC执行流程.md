![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=N5vfq&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 从源码角度看执行流程
以下是核心代码：
```java
public class DispatcherServlet extends FrameworkServlet {
    protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
        // 根据请求对象request获取
        // 这个对象是在每次发送请求时都创建一个，是请求级别的
        // 该对象中描述了本次请求应该执行的拦截器是哪些，顺序是怎样的，要执行的处理器是哪个
        HandlerExecutionChain mappedHandler = getHandler(processedRequest);

        // 根据处理器获取处理器适配器。（底层使用了适配器模式）
        // HandlerAdapter在web服务器启动的时候就创建好了。（启动时创建多个HandlerAdapter放在List集合中）
        // HandlerAdapter有多种类型：
        // RequestMappingHandlerAdapter：用于适配使用注解 @RequestMapping 标记的控制器方法
        // SimpleControllerHandlerAdapter：用于适配实现了 Controller 接口的控制器
        // 注意：此时还没有进行数据绑定（也就是说，表单提交的数据，此时还没有转换为pojo对象。）
        HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

        // 执行请求对应的所有拦截器中的 preHandle 方法
        if (!mappedHandler.applyPreHandle(processedRequest, response)) {
            return;
        }

        // 通过处理器适配器调用处理器方法
        // 在调用处理器方法之前会进行数据绑定，将表单提交的数据绑定到处理器方法上。（底层是通过WebDataBinder完成的）
        // 在数据绑定的过程中会使用到消息转换器：HttpMessageConverter
        // 结束后返回ModelAndView对象
        mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

        //  执行请求对应的所有拦截器中的 postHandle 方法
        mappedHandler.applyPostHandle(processedRequest, response, mv);

        // 处理分发结果（在这个方法中完成了响应）
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }

    // 根据每一次的请求对象来获取处理器执行链对象
    protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
		if (this.handlerMappings != null) {
            // HandlerMapping在服务器启动的时候就创建好了，放到了List集合中。HandlerMapping也有多种类型
            // RequestMappingHandlerMapping：将 URL 映射到使用注解 @RequestMapping 标记的控制器方法的处理器。
            // SimpleUrlHandlerMapping：将 URL 映射到处理器中指定的 URL 或 URL 模式的处理器。
			for (HandlerMapping mapping : this.handlerMappings) {
                // 重点：这是一次请求的开始，实际上是通过处理器映射器来获取的处理器执行链对象
                // 底层实际上会通过 HandlerMapping 对象获取 HandlerMethod对象，将HandlerMethod 对象传递给 HandlerExecutionChain对象。
                // 注意：HandlerMapping对象和HandlerMethod对象都是在服务器启动阶段创建的。
                // RequestMappingHandlerMapping对象中有多个HandlerMethod对象。
				HandlerExecutionChain handler = mapping.getHandler(request);
				if (handler != null) {
					return handler;
				}
			}
		}
		return null;
	}

    private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {
        // 渲染
        render(mv, request, response);
        // 渲染完毕后，调用该请求对应的所有拦截器的 afterCompletion方法。
        mappedHandler.triggerAfterCompletion(request, response, null);
    }

    protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
        // 通过视图解析器返回视图对象
        view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
        // 真正的渲染视图
        view.render(mv.getModelInternal(), request, response);
    }

    protected View resolveViewName(String viewName, @Nullable Map<String, Object> model,
			Locale locale, HttpServletRequest request) throws Exception {
        // 通过视图解析器返回视图对象
        View view = viewResolver.resolveViewName(viewName, locale);
	}
}
```
```java
public interface ViewResolver {
    View resolveViewName(String viewName, Locale locale) throws Exception;
}
```
```java
public interface View {
    void render(@Nullable Map<String, ?> model, HttpServletRequest request, HttpServletResponse response)
			throws Exception;
}
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=Ezc0N&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)

# 从图片角度看执行流程
![未命名文件.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711943505835-476f954e-ba6c-4a78-b16b-683524e25520.png#averageHue=%23292929&clientId=u47cca508-d3a6-4&from=paste&height=529&id=u96674032&originHeight=529&originWidth=1508&originalType=binary&ratio=1&rotation=0&showTitle=false&size=106062&status=done&style=none&taskId=ue5871306-6160-49ea-adff-824a6047704&title=&width=1508)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=noTVy&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# WEB服务器启动时都做了什么
![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=u48f9f116&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
先搞明白核心类的继承关系：
DispatcherServlet extends FrameworkServlet extends HttpServletBean extends HttpServlet extends GenericServlet implements Servlet

服务器启动阶段完成了：

1. 初始化Spring上下文，也就是创建所有的bean，让IoC容器将其管理起来。
2. 初始化SpringMVC相关的对象：处理器映射器，处理器适配器等。。。


![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945073073-1466293a-37a5-4e04-a628-00225ec9ad8f.png#averageHue=%23312b2b&clientId=u47cca508-d3a6-4&from=paste&height=329&id=u358e4869&originHeight=329&originWidth=1035&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41768&status=done&style=none&taskId=ue0e7b33f-2040-48d3-9f7f-b14b11f7f33&title=&width=1035)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945189838-6546c84c-23c9-479d-b2df-893851fdb912.png#averageHue=%23312c2b&clientId=u47cca508-d3a6-4&from=paste&height=482&id=u1c451def&originHeight=482&originWidth=842&originalType=binary&ratio=1&rotation=0&showTitle=false&size=61704&status=done&style=none&taskId=u8acf73eb-3cc3-4cdf-80b9-c044b98647a&title=&width=842)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945264590-8b563ba5-bf2a-4e27-8695-9a0ee2577f2a.png#averageHue=%232d2c2b&clientId=u47cca508-d3a6-4&from=paste&height=732&id=udcc03fa7&originHeight=732&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=113073&status=done&style=none&taskId=u7278ee3d-40e5-4aff-8a6c-1fcd9b41fe8&title=&width=1280)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=exKyU&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)


![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945298853-016466d1-3882-461f-8ac5-296983a67d24.png#averageHue=%232d2c2b&clientId=u47cca508-d3a6-4&from=paste&height=854&id=ufef5ee9d&originHeight=854&originWidth=1348&originalType=binary&ratio=1&rotation=0&showTitle=false&size=146816&status=done&style=none&taskId=u7916a24c-9837-4c7c-aaeb-e0eabc2b23c&title=&width=1348)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=OHfRq&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)


![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945338150-b4f14a20-cc75-4915-9651-51acbffcd872.png#averageHue=%23736147&clientId=u47cca508-d3a6-4&from=paste&height=256&id=u3f47fae4&originHeight=256&originWidth=805&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44737&status=done&style=none&taskId=u65839005-9f4b-4277-9d92-b61b768aa03&title=&width=805)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945352375-01882059-ab91-4668-a595-eb83ca01344c.png#averageHue=%23655544&clientId=u47cca508-d3a6-4&from=paste&height=264&id=u935e0db6&originHeight=264&originWidth=815&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35230&status=done&style=none&taskId=ufc69e66f-7837-4fa1-8cad-3a9b2fe9109&title=&width=815)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945371377-87ac618e-495f-4fe9-92c4-50a1f2c199d8.png#averageHue=%23675a43&clientId=u47cca508-d3a6-4&from=paste&height=192&id=u63c8bc73&originHeight=192&originWidth=757&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23391&status=done&style=none&taskId=u3cac4c60-a51e-4fb7-911f-74ced3d83c6&title=&width=757)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=MUy7B&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)


![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945408231-6e96abeb-ceff-480e-9f2c-72bfa2a5d419.png#averageHue=%23302d2c&clientId=u47cca508-d3a6-4&from=paste&height=577&id=u9d958078&originHeight=577&originWidth=748&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80827&status=done&style=none&taskId=u81fde2bc-f1cc-4d5b-acf9-6b8a3c3a9ae&title=&width=748)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=g9qiR&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
