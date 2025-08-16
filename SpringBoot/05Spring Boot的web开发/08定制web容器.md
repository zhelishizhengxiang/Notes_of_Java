
## 1.web服务器切换为jetty
* **springboot默认嵌入的web服务器是Tomcat，如何切换到jetty服务器？
* **实现方式：排除Tomcat，添加Jetty依赖**
* **修改 `pom.xml` 文件**：在 `pom.xml` 中，确保你使用 `spring-boot-starter-web` 并排除 Tomcat，然后添加 Jetty 依赖。

```xml
<!-- 排除 Tomcat -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!-- 添加 Jetty 依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>

```

## 2.web服务器切换原理
从哪里可以看出springboot是直接将tomcat服务器嵌入到应用中的呢？看这个类：`ServletWebServerFactoryAutoConfiguration`

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731572949358-481582b6-0e79-4f4c-b556-b1ec3c13882c.png)

以上代码显示嵌入的是3个服务器。但并不是都生效，我们来看一下生效条件：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573044414-b1cb4767-de21-4897-9d38-36e6382a8581.png)

* **生效条件是，看类路径当中是否有对应服务器相关的类，如果有则生效**。`spring-boot-web-starter`这个web启动器引入的时候，大家都知道，它间接引入的是tomcat服务器的jar包。因此默认Tomcat服务器被嵌入。如果想要切换web服务器，将tomcat相关jar包排除掉，引入jetty的jar包之后，jetty服务器就会生效，这就是切换web服务器的原理。

## 3.web服务器优化
通过以下源码得知，web服务器的相关配置和`ServerProperties`有关系：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573299936-414ae5c8-87a8-4810-b448-ac1b67f3cfb2.png)

查看`ServerProperties`源码：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573337370-7d40853c-9c5a-4750-a40d-cf00d1528fb4.png)

得知web服务器的配置都是以`server`开头的。



那么如果要配置tomcat服务器怎么办？要配置jetty服务器怎么办？请看一下源码

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573416792-229c5dbb-4a42-4037-934d-bd004bc46a7d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1731573452454-a150e6b1-6fa0-411c-b2a5-063f5b03be6b.png)

* **通过以上源码得知，如果要对tomcat服务器进行配置，前缀为：`server.tomcat`**
* **如果要对jetty服务器进行配置，前缀为：`server.jetty`**。



在以后的开发中关于tomcat服务器的常见优化配置有：

```properties
# 这个参数决定了 Tomcat 在接收请求时，如果在指定的时间内没有收到完整的请求数据，将会关闭连接。这个超时时间是从客户端发送请求开始计算的。
# 防止长时间占用资源：如果客户端发送请求后，长时间没有发送完所有数据，Tomcat 会在这个超时时间内关闭连接，从而释放资源，避免资源被长时间占用。
server.tomcat.connection-timeout=20000

# 设置 Tomcat 服务器处理请求的最大线程数为 200。
# 如果超过这个数量的请求同时到达，Tomcat 会将多余的请求放入一个等待队列中。
# 如果等待队列也满了（由 server.tomcat.accept-count 配置），新的请求将被拒绝，通常会返回一个“503 Service Unavailable”错误。
server.tomcat.max-threads=200

# 用来设置等待队列的最大容量
server.tomcat.accept-count=100

# 设置 Tomcat 服务器在空闲时至少保持 10 个线程处于活动状态，以便快速响应新的请求。
server.tomcat.min-spare-threads=10

# 允许 Tomcat 服务器在关闭后立即重新绑定相同的地址和端口，即使该端口还在 TIME_WAIT 状态
# 当一个网络连接关闭时，操作系统会将该连接的端口保持在 TIME_WAIT 状态一段时间（通常是 2-4 分钟），以确保所有未完成的数据包都能被正确处理。在这段时间内，该端口不能被其他进程绑定。
server.tomcat.address-reuse-enabled=true

# 设置 Tomcat 服务器绑定到所有可用的网络接口，使其可以从任何网络地址访问。
server.tomcat.bind-address=0.0.0.0

# 设置 Tomcat 服务器使用 HTTP/1.1 协议处理请求。
server.tomcat.protocol=HTTP/1.1

# 设置 Tomcat 服务器的会话(session)超时时间为 30 分钟。具体来说，如果用户在 30 分钟内没有与应用进行任何交互，其会话将被自动注销。
server.tomcat.session-timeout=30

# 设置 Tomcat 服务器的静态资源缓存时间为 3600 秒（即 1 小时），这意味着浏览器会在 1 小时内缓存这些静态资源，减少重复请求。
server.tomcat.resource-cache-period=3600

# 解决get请求乱码。对请求行url进行编码。
server.tomcat.uri-encoding=UTF-8

# 设置 Tomcat 服务器的基础目录为当前工作目录（. 表示当前目录）。这个配置指定了 Tomcat 服务器的工作目录，包括日志文件、临时文件和其他运行时生成的文件的存放位置。 生产环境中可能需要重新配置。
server.tomcat.basedir=.
```

