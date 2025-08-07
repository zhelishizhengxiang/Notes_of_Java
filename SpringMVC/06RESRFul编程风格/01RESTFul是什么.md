## 1.RESTFul是什么
* **RESTFul是`WEB服务接口`的一种设计风格。**
* **RESTFul定义了一组约束条件和规范，可以让`WEB服务接口`更加简洁、易于理解、易于扩展、安全可靠。**

1. RESTFul对一个`WEB服务接口`都规定了哪些东西？
	- **对请求的URL格式有约束和规范**
	- **对HTTP的请求方式有约束和规范**
	- 对请求和响应的数据格式有约束和规范
	- 对HTTP状态码有约束和规范
	- 等 ......
2. **REST对请求方式的约束是这样的**：
	- **查询必须发送GET请求**
	- **新增必须发送POST请求**
	- **修改必须发送PUT请求**
	- **删除必须发送DELETE请求**
3. **REST对URL的约束是这样的**：
	- **传统的URL：get请求，/springmvc/getUserById?id=1**
	- **REST风格的URL：get请求，/springmvc/user/1**

	- 传统的URL：get请求，/springmvc/deleteUserById?id=1
	- REST风格的URL：delete请求, /springmvc/user/1

* RESTFul对URL的约束和规范的核心是：**通过采用**`**不同的请求方式**`**+ **`**URL**`**来确定WEB服务中的资源。**
* RESTful 的英文全称是 Representational State Transfer（表述性状态转移）。简称REST。
	* 表述性（Representational）是：URI + 请求方式。
	* 状态（State）是：服务器端的数据。
	* 转移（Transfer）是：变化。
	* **表述性状态转移是指：通过 URI + 请求方式 来控制服务器端数据的变化**。


## 2.RESTFul风格与传统方式对比
传统的 URL 与 RESTful URL 的区别是传统的 URL 是基于方法名进行资源访问和操作，而 RESTful URL 是基于资源的结构和状态进行操作的。下面是一张表格，展示两者之间的具体区别：

| **传统的 URL** | **RESTful URL** |
| --- | --- |
| GET /getUserById?id=1 | GET /user/1 |
| GET /getAllUser | GET /user |
| POST /addUser | POST /user |
| POST /modifyUser | PUT /user |
| GET /deleteUserById?id=1 | DELETE /user/1 |

从上表中我们可以看出，**传统的URL是基于动作的，而 RESTful URL 是基于资源和状态的**，因此 RESTful URL 更加清晰和易于理解，这也是 REST 架构风格被广泛使用的主要原因之一。

