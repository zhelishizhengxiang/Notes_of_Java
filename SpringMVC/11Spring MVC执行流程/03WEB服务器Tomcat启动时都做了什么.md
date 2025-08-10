
* 先搞明白核心类的继承关系：**DispatcherServlet extends FrameworkServlet extends HttpServletBean extends HttpServlet extends GenericServlet implements Servlet**


注意tomcat与DispatchServlet的关系：

![](assets/03WEB服务器Tomcat启动时都做了什么/file-20250808184151159.png)
![](assets/03WEB服务器Tomcat启动时都做了什么/file-20250808184300001.png)

**服务器启动阶段完成了：**
1. **初始化Spring上下文，也就是创建所有的bean，让IoC容器将其管理起来**。
2. **初始化SpringMVC相关的对象：处理器映射器，处理器适配器等**。


![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945073073-1466293a-37a5-4e04-a628-00225ec9ad8f.png#averageHue=%23312b2b&clientId=u47cca508-d3a6-4&from=paste&height=329&id=u358e4869&originHeight=329&originWidth=1035&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41768&status=done&style=none&taskId=ue0e7b33f-2040-48d3-9f7f-b14b11f7f33&title=&width=1035)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945189838-6546c84c-23c9-479d-b2df-893851fdb912.png#averageHue=%23312c2b&clientId=u47cca508-d3a6-4&from=paste&height=482&id=u1c451def&originHeight=482&originWidth=842&originalType=binary&ratio=1&rotation=0&showTitle=false&size=61704&status=done&style=none&taskId=u8acf73eb-3cc3-4cdf-80b9-c044b98647a&title=&width=842)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945264590-8b563ba5-bf2a-4e27-8695-9a0ee2577f2a.png#averageHue=%232d2c2b&clientId=u47cca508-d3a6-4&from=paste&height=732&id=udcc03fa7&originHeight=732&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=113073&status=done&style=none&taskId=u7278ee3d-40e5-4aff-8a6c-1fcd9b41fe8&title=&width=1280)



![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945298853-016466d1-3882-461f-8ac5-296983a67d24.png#averageHue=%232d2c2b&clientId=u47cca508-d3a6-4&from=paste&height=854&id=ufef5ee9d&originHeight=854&originWidth=1348&originalType=binary&ratio=1&rotation=0&showTitle=false&size=146816&status=done&style=none&taskId=u7916a24c-9837-4c7c-aaeb-e0eabc2b23c&title=&width=1348)



![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945338150-b4f14a20-cc75-4915-9651-51acbffcd872.png#averageHue=%23736147&clientId=u47cca508-d3a6-4&from=paste&height=256&id=u3f47fae4&originHeight=256&originWidth=805&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44737&status=done&style=none&taskId=u65839005-9f4b-4277-9d92-b61b768aa03&title=&width=805)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945352375-01882059-ab91-4668-a595-eb83ca01344c.png#averageHue=%23655544&clientId=u47cca508-d3a6-4&from=paste&height=264&id=u935e0db6&originHeight=264&originWidth=815&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35230&status=done&style=none&taskId=ufc69e66f-7837-4fa1-8cad-3a9b2fe9109&title=&width=815)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945371377-87ac618e-495f-4fe9-92c4-50a1f2c199d8.png#averageHue=%23675a43&clientId=u47cca508-d3a6-4&from=paste&height=192&id=u63c8bc73&originHeight=192&originWidth=757&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23391&status=done&style=none&taskId=u3cac4c60-a51e-4fb7-911f-74ced3d83c6&title=&width=757)



![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711945408231-6e96abeb-ceff-480e-9f2c-72bfa2a5d419.png#averageHue=%23302d2c&clientId=u47cca508-d3a6-4&from=paste&height=577&id=u9d958078&originHeight=577&originWidth=748&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80827&status=done&style=none&taskId=u81fde2bc-f1cc-4d5b-acf9-6b8a3c3a9ae&title=&width=748)


