
* **请求域：reques**t
* **会话域：session**
* **应用域：application**
三个域都有以下三个方法：
```java
// 向域中存储数据
void setAttribute(String name, Object obj);

// 从域中读取数据
Object getAttribute(String name);

// 删除域中的数据
void removeAttribute(String name);
```
**主要是通过：setAttribute + getAttribute方法来完成在域中数据的传递和共享。**


## 1.request
* **接口名：HttpServletRequest**
* 简称：request
* **request对象代表了一次请求。一次请求一个request。**
* 使用请求域的业务场景：(**请求转发**)在A资源中通过转发的方式跳转到B资源，因为是转发，因此从A到B是一次请求，如果想让A资源和B资源共享同一个数据，可以将数据存储到request域中。

## 2.session
 * **接口名：HttpSession**
* 简称：session
* **session对象代表了一次会话。从打开浏览器开始访问，到最终浏览器关闭，这是一次完整的会话**
* 每个会话session对象都对应一个JSESSIONID，而JSESSIONID生成后以cookie的方式存储在浏览器客户端。浏览器关闭，JSESSIONID失效，会话结束。
* 使用会话域的业务场景：  
	1. **在A资源中通过重定向的方式跳转到B资源，因为是重定向，因此从A到B是两次请求，如果想让A资源和B资源共享同一个数据，可以将数据存储到session域中**。
	2. **登录成功后保存用户的登录状态**。



## 3.application
* **接口名：ServletContext**
* 简称：application  
* **application对象代表了整个web应用，服务器启动时创建，服务器关闭时销毁。对于一个web应用来说，application对象只有一个**。
* **使用应用域的业务场景：记录网站的在线人数。**
