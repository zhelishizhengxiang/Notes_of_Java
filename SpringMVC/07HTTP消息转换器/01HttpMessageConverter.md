
* **HttpMessageConverter是Spring MVC中非常重要的一个接口。翻译为：HTTP消息转换器**。该接口下提供了很多实现类，不同的实现类有不同的转换方式。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711000445139-8bc9f74d-6ec3-4942-8063-5a130eac64eb.png#averageHue=%23e3e6be&clientId=uc1aa87e7-9dc5-4&from=paste&height=630&id=u739b94f0&originHeight=630&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=183370&status=done&style=shadow&taskId=ucf6e1b45-5eaa-4332-ab78-87e4fa043f1&title=&width=640)
* **`StringHttpMessageConverter`是一个默认的消息转换器，没有指定的话默认是该转换器**
* **`FormHttpMessageConverter`是针对于表单，也就是针对于请求体的的消息转换器**
* **`MappingJackson2HttpMessageConverter`是针对于JSON字符串的消息转换器**

## 1.什么是HTTP消息
* **HTTP消息其实就是HTTP协议。HTTP协议包括请求协议和响应协议**。

以下是一份HTTP POST请求协议：
```
POST /springmvc/user/login HTTP/1.1																												--请求行
Content-Type: application/x-www-form-urlencoded																						--请求头
Content-Length: 32
Host: www.example.com
User-Agent: Mozilla/5.0
Connection: Keep-Alive
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
                                                                                          --空白行
username=admin&password=1234																															--请求体
```
以下是一份HTTP GET请求协议：
```
GET /springmvc/user/del?id=1&name=zhangsan HTTP/1.1																				--请求行
Host: www.example.com																																			--请求头
User-Agent: Mozilla/5.0
Connection: Keep-Alive
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
```
以下是一份HTTP响应协议：
```
HTTP/1.1 200 OK																																					--状态行
Date: Thu, 01 Jul 2021 06:35:45 GMT																											--响应头
Content-Type: text/plain; charset=utf-8
Content-Length: 12
Connection: keep-alive
Server: Apache/2.4.43 (Win64) OpenSSL/1.1.1g
                                                                                        --空白行
<!DOCTYPE html>																																					--响应体
<html>
  <head>
    <title>hello</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```


## 2.转换器转换的是什么

**转换的是`HTTP协议`与`Java程序中的对象`之间的互相转换。**

请看下图：   
![无标题.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711002146899-deaef9c8-a3b7-425e-97b1-6ada5477c674.png#averageHue=%23fbfbfb&clientId=uc1aa87e7-9dc5-4&from=paste&height=581&id=u0dac3ab1&originHeight=581&originWidth=692&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41637&status=done&style=shadow&taskId=ud9c7db5e-0b6a-4d34-a2cd-af50a96810e&title=&width=692)
* **上图是我们之前经常写的代码。请求体中的数据是如何转换成user对象的，底层实际上使用了`HttpMessageConverter`接口的其中一个实现类`FormHttpMessageConverter`**。
* **通过上图可以看出`FormHttpMessageConverter`是负责将`请求协议`转换为`Java对象`的**。

再看下图：     
![无标题.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711003362257-f736f7c8-4d55-4e3f-b8f8-cfbab97c21f4.png#averageHue=%23fbfbfb&clientId=uc1aa87e7-9dc5-4&from=paste&height=460&id=u20f64128&originHeight=460&originWidth=1540&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49114&status=done&style=shadow&taskId=uec4ad039-ac13-4c49-b826-2bec563b880&title=&width=1540)
* 上图的代码也是之前我们经常写的，Controller返回值看做逻辑视图名称，视图解析器将其转换成物理视图名称，生成视图对象，**`StringHttpMessageConverter`负责将视图对象中的HTML字符串写入到HTTP协议的响应体中。最终完成响应**。
* **通过上图可以看出`StringHttpMessageConverter`是负责将`Java对象`转换为`响应协议`的**。




通过以上内容的学习，大家应该能够了解到`HttpMessageConverter`接口是用来做什么的了：  
![无标题.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1711003929875-072161b4-af27-4855-9980-5d8ba186730b.png#averageHue=%23ececeb&clientId=uc1aa87e7-9dc5-4&from=paste&height=109&id=ud67955dd&originHeight=109&originWidth=1210&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7784&status=done&style=shadow&taskId=u9f2251ef-a356-46d9-9dfe-52f86b79c11&title=&width=1210)
* **如上图所示：HttpMessageConverter接口的可以将请求协议转换成Java对象，也可以把Java对象转换为响应协议**。
* **HttpMessageConverter是接口，SpringMVC帮我们提供了非常多而丰富的实现类。每个实现类都有自己不同的转换风格。**
* **对于我们程序员来说，Spring MVC已经帮助我们写好了，我们只需要在不同的业务场景下，选择合适的HTTP消息转换器即可。**
* **怎么选择呢？当然是通过SpringMVC为我们提供的注解，我们通过使用不同的注解来启用不同的消息转换器。**

**==在HTTP消息转换器这一小节，我们重点要掌握的是两个注解两个类==**：
- **@ResponseBody**
- **@RequestBody**
- **ResponseEntity**
- **RequestEntity**

