首先@提供RequestMapping的源码

![](assets/03RequestMapping注解属性获取/file-20250804173245086.png)
# 一、RequestMapping注解的value属性
## 1.value属性的使用
* value属性是该注解最核心的属性，**value属性填写的是请求路径，也就是说通过该请求路径与对应的控制器的方法绑定在一起。另外通过源码可以看到value属性是一个字符串数组：**   
![](assets/03RequestMapping注解属性获取/file-20250804173843162.png)
* **既然是数组，就表示可以提供多个路径，也就是说，在SpringMVC中，多个不同的请求路径可以映射同一个控制器的同一个方法**
* **通过阅读源码可知，value()属性还有一个别名是path()，所以也可以使用path()来指定对应请求路径**。只不过path属性没办法直接省略，所以大多数情况下都是适用value。

编写新的控制器：
```java
package com.powernode.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: RequestMappingTestController
 * Description: 测试 RequestMapping 注解
 * Datetime: 2024/3/14 9:14
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class RequestMappingTestController {
    @RequestMapping(value = {"/testValue1", "/testValue2"})
    public String testValue(){
        return "testValue";
    }
}

```
提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>test Value</title>
</head>
<body>
<h1>Test RequestMapping's Value</h1>
</body>
</html>
```
在index.html文件中添加两个超链接：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>index page</title>
</head>
<body>
<h1>index page</h1>
<a th:href="@{/user/detail}">用户详情</a><br>
<a th:href="@{/product/detail}">商品详情</a><br>

<!--测试RequestMapping的value属性-->
<a th:href="@{/testValue1}">testValue1</a><br>
<a th:href="@{/testValue2}">testValue2</a><br>

</body>
</html>
```
启动服务器，测试，点击以下的两个超链接，发送请求，都可以正常访问到同一个控制器上的同一个方法：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710380856084-a7199701-367e-49d4-856c-843902882df4.png#averageHue=%23f9f9f8&clientId=u4c6f25a9-3fa7-4&from=paste&height=304&id=u53a1c730&originHeight=304&originWidth=359&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13782&status=done&style=shadow&taskId=udeae1893-4115-425d-8bf0-379b0d9ba9b&title=&width=359)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710380869186-247c7c9d-4fa7-4896-91ac-16c227cf0751.png#averageHue=%23d7b583&clientId=u4c6f25a9-3fa7-4&from=paste&height=212&id=ucd526467&originHeight=212&originWidth=513&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16167&status=done&style=shadow&taskId=ub5846959-c2eb-4a9a-94b8-6bda74ed196&title=&width=513)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710380880908-39caa3a2-020d-4f4b-821d-9a14ab6cfb03.png#averageHue=%23d7b583&clientId=u4c6f25a9-3fa7-4&from=paste&height=204&id=u2e13e589&originHeight=204&originWidth=512&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16005&status=done&style=shadow&taskId=u29af56cd-b832-411f-b510-58783f17ef8&title=&width=512)

## 2.Ant风格的value
**value是可以用来匹配路径的，==路径支持模糊匹配==，我们把这种模糊匹配称之为Ant风格**。关于路径中的通配符包括：
- **?，代表任意一个字符（除\/、\?之外的任意字符）**
- **\*，代表0到N个任意字符（除\/、\?之外的任意字符）。路径中不可以出现路径分隔符 /**
- **\*\*，代表0到N个任意字符，并且路径中可以出现路径分隔符 /**。
	- **注意：/x\*\*z/ 实际上并没有使用通配符 ，本质上还是使用的 \*，因为通配符 \*\* 在使用的时候，左右两边都不能有任何字符，必须是 \/。** **即：\*\* 通配符在使用时，左右不能出现字符，只能是 /**
	- 注意：如果使用Spring5以及之前的版本，这样写是没问题的：@RequestMapping(value = "/\*\*/testAntValue") 。 如果使用Spring6以及之后的版本，这样写是报错的。**在Spring6当中，\*\* 通配符只能作为路径的末尾出现。**


测试一下这些通配符，在 RequestMappingTestController 中添加以下方法：
```java
@RequestMapping("/x?z/testValueAnt")
public String testValueAnt(){
    return "testValueAnt";
}
```
提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>test Value Ant</title>
</head>
<body>
<h1>测试RequestMapping注解的value属性支持模糊匹配</h1>
</body>
</html>
```
在index.html页面中编写超链接：
```html
<!--测试RequestMapping注解的value属性支持模糊匹配-->
<a th:href="@{/xyz/testValueAnt}">测试value属性的模糊匹配</a><br>
```
测试结果如下：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408304774-c8fdaa73-4aad-43b2-a0b5-27600e45078b.png#averageHue=%23f9f8f7&clientId=u9e0c3730-20d1-4&from=paste&height=196&id=u81d8f0f8&originHeight=196&originWidth=332&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8387&status=done&style=shadow&taskId=uc0b41afd-6758-4170-b4a7-c8fb4f6c1e8&title=&width=332)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408334347-3ecdd6de-4281-4dda-a31e-bb267496bf76.png#averageHue=%23e9e8e6&clientId=u9e0c3730-20d1-4&from=paste&height=161&id=ud3097a67&originHeight=161&originWidth=801&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23105&status=done&style=shadow&taskId=ubeaa9b73-45be-4ad1-a667-ddd492e4d24&title=&width=801)



通过修改浏览器地址栏上的路径，可以反复测试通配符 ? 的语法：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408441950-b058d7cc-5d1a-42c0-9188-57d294e36c05.png#averageHue=%23ebeae8&clientId=u9e0c3730-20d1-4&from=paste&height=171&id=udb55bc6f&originHeight=171&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23361&status=done&style=shadow&taskId=ue28e8160-c985-447c-817a-e74299a39e1&title=&width=813)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408497513-91234ca4-74ae-4681-87b6-066bf68cb60d.png#averageHue=%23eae8e7&clientId=u9e0c3730-20d1-4&from=paste&height=167&id=u006e21d6&originHeight=167&originWidth=789&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23129&status=done&style=shadow&taskId=u3d5efc45-4431-476d-aa86-445cf51d4d6&title=&width=789)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408554224-46bdf1bb-0fb9-4214-b30c-9b9a73973ff7.png#averageHue=%23e8e6e5&clientId=u9e0c3730-20d1-4&from=paste&height=156&id=ufc167ac5&originHeight=156&originWidth=786&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22991&status=done&style=shadow&taskId=uc0692ffc-ac7e-46f6-a9cd-c8ef334e9c8&title=&width=786)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408520959-fdf3be9b-341c-4f9f-9d48-0b1b76e1b5b3.png#averageHue=%23e6c692&clientId=u9e0c3730-20d1-4&from=paste&height=276&id=u6694dbe1&originHeight=276&originWidth=562&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21138&status=done&style=shadow&taskId=ucfb93bfa-7af8-4e60-b763-619def0d1fb&title=&width=562)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408535497-902d0a9a-8fbf-4b9d-b171-a6fa5b425900.png#averageHue=%23e6c692&clientId=u9e0c3730-20d1-4&from=paste&height=286&id=u0149d972&originHeight=286&originWidth=557&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21035&status=done&style=shadow&taskId=ub8b3dfe9-8bd5-47e9-bccc-b5d7ab5c352&title=&width=557)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408461985-6de127ca-d27f-40af-be89-71f2d1e298f1.png#averageHue=%23e6c794&clientId=u9e0c3730-20d1-4&from=paste&height=274&id=ub4603a17&originHeight=274&originWidth=572&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20722&status=done&style=shadow&taskId=ud8e003d2-1dca-4907-99e2-5243fd6172d&title=&width=572)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710408477041-0b7f3fc7-8ab2-4b1b-acb0-54fa009c5df1.png#averageHue=%23e6c692&clientId=u9e0c3730-20d1-4&from=paste&height=289&id=u844d5d11&originHeight=289&originWidth=577&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21172&status=done&style=shadow&taskId=u17a48bb5-b555-40b7-a3a5-098bb35a22a&title=&width=577)

将 ? 通配符修改为 * 通配符：
```java
//@RequestMapping("/x?z/testValueAnt")
@RequestMapping("/x*z/testValueAnt")
public String testValueAnt(){
    return "testValueAnt";
}
```
打开浏览器直接在地址栏上输入路径进行测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710409236128-4faa78a0-8da7-46b5-a466-58259918354a.png#averageHue=%23e9e7e6&clientId=u9e0c3730-20d1-4&from=paste&height=159&id=udaa45e31&originHeight=159&originWidth=797&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23274&status=done&style=shadow&taskId=u07fa12c6-f90e-473f-be1d-c16415bac39&title=&width=797)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710409281578-57812acc-e94c-441f-91cf-35ed19c0912d.png#averageHue=%23e9e7e6&clientId=u9e0c3730-20d1-4&from=paste&height=155&id=u8c48fc58&originHeight=155&originWidth=823&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23112&status=done&style=shadow&taskId=ud351839f-0670-48b1-9148-dd142e0c569&title=&width=823)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710409267167-adb48ec7-861c-40f2-8a92-c1a4368de9fe.png#averageHue=%23e5c692&clientId=u9e0c3730-20d1-4&from=paste&height=272&id=u685780a5&originHeight=272&originWidth=556&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21012&status=done&style=shadow&taskId=ub93fc575-2c72-4cf9-8013-b09b5f3d827&title=&width=556)

将 * 通配符修改为 ** 通配符：
```java
@RequestMapping("/x**z/testValueAnt")
public String testValueAnt(){
    return "testValueAnt";
}
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710409419674-7475d2c4-989a-4547-9f8c-a2964b2d7eb7.png#averageHue=%23e5c592&clientId=u9e0c3730-20d1-4&from=paste&height=278&id=ub986b869&originHeight=278&originWidth=600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21339&status=done&style=shadow&taskId=u45bed6ca-c0cd-41d6-aea2-4b396844a19&title=&width=600)


```java
@RequestMapping("/**/testValueAnt")
public String testValueAnt(){
    return "testValueAnt";
}
```
启动服务器发现报错了：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710410631877-81bfcc14-3ead-4f2c-99cf-69e0e39c9b3e.png#averageHue=%23fcfbfa&clientId=u9e0c3730-20d1-4&from=paste&height=176&id=u4e591839&originHeight=176&originWidth=963&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29038&status=done&style=shadow&taskId=u56772db8-8c87-4a36-80c7-6b3ff2a47cf&title=&width=963)
以上写法在Spring5的时候是支持的，但是在Spring6中进行了严格的规定，** 通配符只能出现在路径的末尾，例如：
```java
@RequestMapping("/testValueAnt/**")
public String testValueAnt(){
    return "testValueAnt";
}
```
测试结果：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710410734275-31609763-9ca9-46ec-b8d4-539612055ffe.png#averageHue=%23ebeae8&clientId=u9e0c3730-20d1-4&from=paste&height=176&id=u013d4106&originHeight=176&originWidth=792&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23052&status=done&style=shadow&taskId=u78626697-57e7-46e2-b00d-97253913ce6&title=&width=792)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710410746239-dcb5b607-28e9-4996-88b6-4e94b411cc6f.png#averageHue=%23eae9e7&clientId=u9e0c3730-20d1-4&from=paste&height=168&id=ud86c7b22&originHeight=168&originWidth=796&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23320&status=done&style=shadow&taskId=u45c623ea-dfc9-4f24-bbcf-bf40caf96c4&title=&width=796)

## 3.value中的占位符（重点）
* 到目前为止，我们的请求路径是这样的格式：uri?name1=value1&name2=value2&name3=value3
* 其实除了这种方式，**还有另外一种格式的请求路径，格式为：uri/value1/value2/value3，我们将这样的请求路径叫做 RESTful 风格的请求路径。RESTful风格的请求路径在现代的开发中使用较多。**
* **普通的请求路径：http://localhost:8080/springmvc/login?username=admin&password=123&age=20。RESTful风格的请求路径：http://localhost:8080/springmvc/login/admin/123/20**（后期会专门讲，这里知道有这么回事就行）
* 如果使用RESTful风格的请求路径，**在控制器中应该如何获取请求中的数据呢？**  
	1. **可以在value属性中使用占位符，例如：/login/{id}/{username}/{password}**
	2. **同时在绑定的方法的参数中使用`@PathVariable("占位符") String username` ，代表把占位符的获得到的值赋给该参数**

在 RequestMappingTestController 类中添加一个方法：
```java
@RequestMapping(value="/testRESTful/{id}/{username}/{age}")
public String testRESTful(
        @PathVariable("id")
        int id,
        @PathVariable("username")
        String username,
        @PathVariable("age")
        int age){
    System.out.println(id + "," + username + "," + age);
    return "testRESTful";
}
```
提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>test RESTful</title>
</head>
<body>
<h1>测试value属性使用占位符</h1>
</body>
</html>
```
在 index.html 页面中添加超链接：
```html
<!--测试RequestMapping注解的value属性支持占位符-->
<a th:href="@{/testRESTful/1/zhangsan/20}">测试value属性使用占位符</a>
```

启动服务器测试：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710414703219-b27d6ea5-cbee-4e42-a11d-cb743563507e.png#averageHue=%23fbfafa&clientId=u9e0c3730-20d1-4&from=paste&height=296&id=u0bd8dc6d&originHeight=296&originWidth=462&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17171&status=done&style=shadow&taskId=u3727469b-f711-42f3-8eee-a36f4680f07&title=&width=462)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710414717194-86932dc5-5c6c-46b5-acb0-ab04778051ad.png#averageHue=%23f0eeed&clientId=u9e0c3730-20d1-4&from=paste&height=166&id=uee30dc98&originHeight=166&originWidth=569&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15507&status=done&style=shadow&taskId=u7098ad24-11d7-4490-9539-0d4f1546577&title=&width=569)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710414728167-7b3f3348-feb3-4b62-89c4-047c30e6f3ee.png#averageHue=%23f8f5f2&clientId=u9e0c3730-20d1-4&from=paste&height=161&id=ud5db4289&originHeight=161&originWidth=481&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21589&status=done&style=shadow&taskId=ub0aeabca-e5be-4a8c-967c-f2097635448&title=&width=481)


# 二、RequestMapping注解的method属性
## 1.method属性的作用
在Servlet当中，如果后端要求前端必须发送一个post请求，后端可以通过重写doPost方法来实现。后端要求前端必须发送一个get请求，后端可以通过重写doGet方法来实现。当重写的方法是doPost时，前端就必须发送post请求，当重写doGet方法时，前端就必须发送get请求。如果前端发送请求的方式和后端的处理方式不一致时，会出现405错误。

HTTP状态码405，这种机制的作用是：限制客户端的请求方式，以保证服务器中数据的安全。

* **假设后端程序要处理的请求是一个登录请求，为了保证登录时的用户名和密码不被显示到浏览器的地址栏上，后端程序有义务要求前端必须发送一个post请求，如果前端发送get请求，则应该拒绝。**
* 那么在SpringMVC框架中应该如何实现这种机制呢？**可以使用RequestMapping注解的method属性来实现。**
* 一句话总结：**当前端发送的请求路径是 /user/login ，并且发送的请求方式是以POST方式请求的。才可以正常映射。请求路径不匹配或者是方法不匹配，都不会映射到该方法上。**

通过RequestMapping源码可以看到，method属性也是一个数组：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710383145104-28befda6-4f03-4cc0-888d-f0c68e802489.png#averageHue=%23fcfaf9&clientId=u4c6f25a9-3fa7-4&from=paste&height=140&id=ub2730852&originHeight=140&originWidth=656&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23040&status=done&style=none&taskId=u46d2d6ff-ccc0-4d6d-9660-3d2cc65d92a&title=&width=656)
数组中的每个元素是 RequestMethod，而RequestMethod是一个枚举类型的数据：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710383181561-c7807a8e-1e03-48bd-93ab-044900f7b52c.png#averageHue=%23fefcf8&clientId=u4c6f25a9-3fa7-4&from=paste&height=152&id=ufc6e6501&originHeight=152&originWidth=730&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13161&status=done&style=none&taskId=u265bb765-09dd-4927-a64f-14dd51af742&title=&width=730)

因此如果要求前端发送POST请求，该注解应该这样用：
```java
@RequestMapping(value = "/login", method = RequestMethod.POST)
public String login(){
    return "success";
}
```

接下来，我们来测试一下：

在RequestMappingTestController类中添加以下方法：
```java
//当前端发送的请求路径是 /user/login ，并且发送的请求方式是以POST方式请求的。才可以正常映射。
@RequestMapping(value="/login", method = RequestMethod.POST)
public String testMethod(){
    return "testMethod";
}
```
提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>test Method</title>
</head>
<body>
<h1>Login Success!!!</h1>
</body>
</html>
```


在index.html页面中提供一个登录的form表单，后端要求发送post请求，则form表单的method属性应设置为post：
```html
<!--测试RequestMapping的method属性-->
<form th:action="@{/login}" method="post">
    用户名：<input type="text" name="username"/><br>
    密码：<input type="password" name="password"/><br>
    <input type="submit" value="登录">
</form>
```
启动服务器，测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710383700474-55cf63ab-7f36-4ab7-b5f7-41046000472d.png#averageHue=%23f9f9f8&clientId=u4c6f25a9-3fa7-4&from=paste&height=374&id=u86e908b5&originHeight=374&originWidth=399&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17366&status=done&style=shadow&taskId=u2fde9e03-0dbd-4ad7-ad13-5a725498b02&title=&width=399)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710383716323-ad5bf478-30e9-48ed-8238-ebacaf395625.png#averageHue=%23f6f5f4&clientId=u4c6f25a9-3fa7-4&from=paste&height=208&id=ud54dba53&originHeight=208&originWidth=414&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13058&status=done&style=shadow&taskId=u1fa2c31f-7e25-4358-b0fc-e62bdb6b5b4&title=&width=414)


通过测试，前端发送的请求方式post，后端处理请求的方式也是post，就不会有问题。
当然，如果后端要求前端必须发送post请求，而前端发送了get请求，则会出现405错误，将index.html中form表单提交方式修改为get：
```html
<!--测试RequestMapping的method属性-->
<form th:action="@{/login}" method="get">
    用户名：<input type="text" name="username"/><br>
    密码：<input type="password" name="password"/><br>
    <input type="submit" value="登录">
</form>
```
再次测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710383866495-ea6560e6-b458-4385-95cb-c9a9b24b08cd.png#averageHue=%23e4c595&clientId=u4c6f25a9-3fa7-4&from=paste&height=323&id=u8c5a2905&originHeight=323&originWidth=614&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25618&status=done&style=shadow&taskId=ubb1d2640-6dbc-4a41-bd71-41b5b162f55&title=&width=614)

**因此，可以看出，对于RequestMapping注解来说，多一个属性，就相当于多了一个映射的条件，如果value和method属性都有，==则表示只有前端发送的请求路径 + 请求方式都满足时才能与控制器上的方法建立映射关系，只要有一个不满足，则无法建立映射关系==。例如：@RequestMapping(value="/login", method = RequestMethod.POST) 表示当前端发送的请求路径是 /login，并且发送请求的方式是POST的时候才会建立映射关系。如果前端发送的是get请求，或者前端发送的请求路径不是 /login，则都是无法建立映射的。**


## 2.衍生Mapping
对于以上的程序来说，SpringMVC提供了另一个注解，使用这个注解更加的方便，它就是：PostMapping，使用该注解时，不需要指定method属性，因为它默认采用的就是POST处理方式。

![](assets/03RequestMapping注解属性获取/file-20250804195437247.png)
*  @PostMapping 注解代替的是：@RequestMapping(value="", method=RequestMethod.POST)  。@GetMapping 注解代替的是：@RequestMapping(value="", method=RequestMethod.GET)  
	....  @PutMapping  @DeleteMapping  @PatchMapping
* 在SpringMVC中不仅提供了 **PostMaping**注解，像这样的注解还有四个，包括：
	- **GetMapping**：要求前端必须发送get请求
	- **PutMapping**：要求前端必须发送put请求
	- **DeleteMapping**：要求前端必须发送delete请求
	- **PatchMapping**：要求前端必须发送patch请求

修改RequestMappingTestController代码如下
```java
//@RequestMapping(value="/login", method = RequestMethod.POST)
@PostMapping("/login")
public String testMethod(){
    return "testMethod";
}
```
当前端发送get请求时，测试一下：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710384745231-3f0f3e3d-e151-4ac8-bde2-e48798aadde0.png#averageHue=%23e4c595&clientId=u4c6f25a9-3fa7-4&from=paste&height=314&id=ue0d397c5&originHeight=314&originWidth=611&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25589&status=done&style=shadow&taskId=ud541dff9-f9ed-4015-88ad-ba0c552399f&title=&width=611)
当前端发送post请求时，测试一下：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710384819897-64de621f-fb7d-495e-98d3-5e7b192c458d.png#averageHue=%23f7f6f6&clientId=u4c6f25a9-3fa7-4&from=paste&height=220&id=u2e7e5b8d&originHeight=220&originWidth=465&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13252&status=done&style=shadow&taskId=u738caa0e-fbf9-4b40-b56d-b4f88ea44ec&title=&width=465)





## 3.web的请求方式
前端向服务器发送请求的方式包括哪些？共9种，**前5种常用，后面作为了解**：
- **GET：获取资源，只允许读取数据，不影响数据的状态和功能。使用 URL 中传递参数或者在 HTTP 请求的头部使用参数，服务器返回请求的资源。**
- **POST：向服务器提交资源，可能还会改变数据的状态和功能。通过表单等方式提交请求体，服务器接收请求体后，进行数据处理。**
- **PUT：更新资源，用于更新指定的资源上所有可编辑内容。通过请求体发送需要被更新的全部内容，服务器接收数据后，将被更新的资源进行替换或修改。**
- **DELETE：删除资源，用于删除指定的资源。将要被删除的资源标识符放在 URL 中或请求体中。**
- **HEAD：请求服务器返回资源的头部，与 GET 命令类似，但是所有返回的信息都是头部信息，不能包含数据体。主要用于资源检测和缓存控制。**
- PATCH：部分更改请求。当被请求的资源是可被更改的资源时，请求服务器对该资源进行部分更新，即每次更新一部分。
- OPTIONS：请求获得服务器支持的请求方法类型，以及支持的请求头标志。“OPTIONS \*”则返回支持全部方法类型的服务器标志。
- TRACE：服务器响应输出客户端的 HTTP 请求，主要用于调试和测试。
- CONNECT：建立网络连接，通常用于加密 SSL/TLS 连接。

**常用的数据库操作对应的请求方式的选用**  
![](assets/03RequestMapping注解属性获取/file-20250804200158674.png)

注意：
1. **使用超链接以及原生的form表单只能提交get和post请求，put、delete、head请求可以使用发送ajax请求的方式来实现**。
2. **使用超链接发送的只能是get请求**
3. **使用form表单，如果没有设置method，发送get请求**
4. **使用form表单，设置method="get"，发送get请求**
5. **使用form表单，设置method="post"，发送post请求**
6. **使用form表单，设置method="put/delete/head"，发送其实还是get请求。（针对这种情况，可以测试一下）**

将index.html中登录表单的提交方式method设置为put：
```html
<!--测试RequestMapping的method属性-->
<form th:action="@{/login}" method="put">
    用户名：<input type="text" name="username"/><br>
    密码：<input type="password" name="password"/><br>
    <input type="submit" value="登录">
</form>
```
修改RequestMappingTestController类的代码：
```java
@RequestMapping(value="/login", method = RequestMethod.PUT)
//@PostMapping("/login")
public String testMethod(){
    return "testMethod";
}
```
测试结果：   
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710387909246-423bd4a6-9e73-40ca-ab7e-fac29f98f61f.png#averageHue=%23e4c595&clientId=u4c6f25a9-3fa7-4&from=paste&height=341&id=u5026a61a&originHeight=341&originWidth=627&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26425&status=done&style=shadow&taskId=u5f6746fb-8fe8-44be-bd0e-d3030ea3cc0&title=&width=627)
通过测试得知，即使form中method设置为put方式，但仍然采用get方式发送请求。
再次修改RequestMappingTestController：
```java
@RequestMapping(value="/login", method = RequestMethod.GET)
//@PostMapping("/login")
public String testMethod(){
    return "testMethod";
}
```
再次测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710388055974-40f19d04-9b29-459e-9821-f330066e1e2c.png#averageHue=%23d5b587&clientId=u4c6f25a9-3fa7-4&from=paste&height=211&id=ua8a0b6a6&originHeight=211&originWidth=589&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14700&status=done&style=shadow&taskId=u42564f22-655c-48d8-a0d6-68ad1e0ddca&title=&width=589)



## 4.GET和POST的区别
在之前发布的JavaWEB视频中对HTTP请求协议的GET和POST进行了详细讲解，这里就不再赘述，大致回顾一下。

HTTP请求协议之GET请求：
```  
GET /springmvc/login?username=lucy&userpwd=1111 HTTP/1.1                           请求行
Host: localhost:8080                                                                    请求头
Connection: keep-alive
sec-ch-ua: "Google Chrome";v="95", "Chromium";v="95", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8080/springmvc/index.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
                                                                                        空白行
                                                                                        请求体
```
HTTP请求协议之POST请求：
```
POST /springmvc/login HTTP/1.1                                                  请求行
Host: localhost:8080                                                                  请求头
Connection: keep-alive
Content-Length: 25
Cache-Control: max-age=0
sec-ch-ua: "Google Chrome";v="95", "Chromium";v="95", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: http://localhost:8080
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8080/springmvc/index.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
                                                                                      空白行
username=lisi&userpwd=123                                                             请求体
```

区别：  
1. get请求发送数据的时候，数据会挂在URI的后面，并且在URI后面添加一个“?”，"?"后面是数据。这样会导致发送的数据回显在浏览器的地址栏上。比如http://localhost:8080/servlet05/getServlet?username=zhangsan&userpwd=1111。post请求发送数据的时候，在请求体当中发送。不会回显到浏览器的地址栏上。也就是说post发送的数据，在浏览器地址栏上看不到。
2. get请求只能发送普通的字符串。并且发送的字符串长度有限制，不同的浏览器限制不同。这个没有明确的规范。get请求无法发送大数据量。post请求可以发送任何类型的数据，包括普通字符串，流媒体等信息：视频、声音、图片。post请求可以发送大数据量，理论上没有长度限制。
3. get请求在W3C中是这样说的：get请求比较适合从服务器端获取数据。post请求在W3C中是这样说的：post请求比较适合向服务器端传送数据。
4. get请求是安全的。因为在正确使用get请求的前提下，get请求只是为了从服务器上获取数据，不会对服务器数据进行修改。post请求是危险的。因为post请求是修改服务器端的资源。
5. get请求支持缓存。 也就是说当第二次发送get请求时，会走浏览器上次的缓存结果，不再真正的请求服务器。（有时需要避免，怎么避免：在get请求路径后添加时间戳）。post请求不支持缓存。每一次发送post请求都会真正的走服务器。

怎么选择：  
1. 如果你是想从服务器上获取资源，建议使用GET请求，如果你这个请求是为了向服务器提交数据，建议使用POST请求。
2. 大部分的form表单提交，都是post方式，因为form表单中要填写大量的数据，这些数据是收集用户的信息，一般是需要传给服务器，服务器将这些数据保存/修改等。
3. 如果表单中有敏感信息，建议使用post请求，因为get请求会回显敏感信息到浏览器地址栏上。（例如：密码信息）
4. 做文件上传，一定是post请求。要传的数据不是普通文本。
5. 其他情况大部分都是使用get请求。

# 三、RequestMapping注解的params属性
## 1.params属性的理解
**params属性用来设置通过请求参数来映射请求。**

对于RequestMapping注解来说：
- value属性是一个数组，只要满足数组中的任意一个路径，就能映射成功
- method属性也是一个数组，只要满足数组中任意一个请求方式，就能映射成功。
- **params属性也是一个数组，不过要求请求参数必须和params数组中要求的==所有参数完全一致后，才能映射成功==。**

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710398311030-55ee91e0-b4d0-4b43-9d65-36a552eb6d3a.png#averageHue=%23fcf6f3&clientId=u9e0c3730-20d1-4&from=paste&height=162&id=u18907e9f&originHeight=162&originWidth=570&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20198&status=done&style=shadow&taskId=u8ad51516-6751-4c99-bbc5-6709f340f9f&title=&width=570)

## 2.params属性的4种用法
1. @RequestMapping(value="/login", params={**"username"**, "password"}) 表示：**请求参数中必须包含 username 和 password，才能与当前标注的方法进行映射。**
2. @RequestMapping(value="/login", params={**"!username"**, "password"}) 表示：**请求参数中不能包含username参数，但必须包含password参数，才能与当前标注的方法进行映射**。
3. @RequestMapping(value="/login", params={**"username=admin"**, "password"}) 表示：**请求参数中必须包含username参数，并且参数的值必须是admin，另外也必须包含password参数，才能与当前标注的方法进行映射。**
4. @RequestMapping(value="/login", params={**"username!=admin"**, "password"}) 表示：**请求参数中必须包含username参数，但参数的值不能是admin，另外也必须包含password参数，才能与当前标注的方法进行映射**。
5. **注意：上述几个例子中只是设置了包含或者不包含上面这两个属性，但是请求的参数包含其他参数也可以，比如请求参数不止这两个，还由一个age，也是可以对应的方法进行映射的**

注意：如果前端提交的参数，和后端要求的请求参数不一致，则出现400错误！！！

**HTTP状态码400的原因：请求参数格式不正确而导致的。**

## 3.测试params属性
在 RequestMappingTestController 类中添加如下方法：
```java
@RequestMapping(value="/testParams", params = {"username", "password"})
public String testParams(){
    return "testParams";
}
```
提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>testParams</title>
</head>
<body>
<h1>测试RequestMapping注解的Params属性</h1>
</body>
</html>
```
在index.html文件中添加超链接(thymeleaf方式)：
```html
<!--测试RequestMapping的params属性-->
<a th:href="@{/testParams(username='admin',password='123')}">测试params属性</a>
```
当然，你也可以这样写：这样写IDEA会报错，但不影响使用。
```html
<a th:href="@{/testParams?username=admin&password=123}">测试params属性</a><br>
```
启动服务器，测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710400506148-f404474f-771b-4fb7-97a8-5a322012fb33.png#averageHue=%23f9f9f8&clientId=u9e0c3730-20d1-4&from=paste&height=362&id=u8efb554c&originHeight=362&originWidth=396&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17562&status=done&style=shadow&taskId=uad6548f2-e415-4643-b792-f5c5e46bb50&title=&width=396)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710400526780-f4691915-7952-4d91-bb5b-704cf40ab6fd.png#averageHue=%23eceae9&clientId=u9e0c3730-20d1-4&from=paste&height=161&id=u446cb135&originHeight=161&originWidth=685&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19801&status=done&style=shadow&taskId=u1a28d260-92bc-4ea6-a33a-4bd2c077fc4&title=&width=685)


假如发送请求时，没有传递username参数会怎样？
```html
<a th:href="@{/testParams(password='123')}">测试params属性</a><br>
```
启动服务器，测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710400622164-d051b747-dbc7-4044-bbfb-2d3e40602b65.png#averageHue=%23fafaf9&clientId=u9e0c3730-20d1-4&from=paste&height=382&id=u0718a0ff&originHeight=382&originWidth=439&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19312&status=done&style=shadow&taskId=u8e31fe95-713c-4bf7-986f-9924b900b21&title=&width=439)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710400640376-f181e4a5-79a3-4a55-a1d9-1d55582102e0.png#averageHue=%23e6c592&clientId=u9e0c3730-20d1-4&from=paste&height=295&id=u807d8a54&originHeight=295&originWidth=846&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25331&status=done&style=shadow&taskId=u72d9c7f6-837e-4be7-a93d-0930dc7e6d1&title=&width=846)

提示无效的请求参数，服务器无法或不会处理当前请求。
params属性剩下的三种情况，自行测试！！！！

# 四、RequestMapping注解的headers属性（用的较少）
## 1.认识headers属性

**headers和params原理相同，用法也相同。**

**当前端提交的请求头信息包含后端要求的请求头部行信息，才能映射成功**。

请求头信息怎么查看？在chrome浏览器中，F12打开控制台，找到Network，可以查看具体的请求协议和响应协议。在请求协议中可以看到请求头信息，例如：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710402265257-e2b13b8d-52e7-4088-842a-4246be3e866a.png#averageHue=%23fcfbfa&clientId=u9e0c3730-20d1-4&from=paste&height=366&id=u0b591958&originHeight=366&originWidth=987&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32052&status=done&style=shadow&taskId=ue61420fa-9ede-48ed-a4cf-230b74daaa8&title=&width=987)

请求头信息和请求参数信息一样，都是键值对形式，例如上图中：  
- Referer: http://localhost:8080/springmvc/  键是Referer，值是http://localhost:8080/springmvc/
- Host: localhost:8080     键是Host，值是localhost:8080
## 2.headers属性的4种用法
1. @RequestMapping(value="/login", headers={**"Referer"**, "Host"}) 表示：**请求头信息中必须包含Referer和Host，才能与当前标注的方法进行映射。**
2. @RequestMapping(value="/login", headers={**"Referer"**, "!Host"}) 表示：**请求头信息中必须包含Referer，但不包含Host，才能与当前标注的方法进行映射。**
3. @RequestMapping(value="/login", headers={**"Referer=http://localhost:8080/springmvc/"**, "Host"}) 表示：**请求头信息中必须包含Referer和Host，并且Referer的值必须是http://localhost:8080/springmvc/，才能与当前标注的方法进行映射**。
4. @RequestMapping(value="/login", headers={**"Referer!=http://localhost:8080/springmvc/"**, "Host"}) 表示：**请求头信息中必须包含Referer和Host，并且Referer的值不是http://localhost:8080/springmvc/，才能与当前标注的方法进行映射**。

注意：**如果前端提交的请求头信息，和后端要求的请求头信息不一致，则出现404错误！！！**

## 3.测试headers属性
在 RequestMappingTestController 类中添加以下方法：
```java
@RequestMapping(value="/testHeaders", headers = {"Referer=http://localhost:8080/springmvc/"})
public String testHeaders(){
    return "testHeaders";
}
```
提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>test Headers</title>
</head>
<body>
<h1>测试RequestMapping注解的headers属性</h1>
</body>
</html>
```
在index.html页面中添加超链接：
```html
<!--测试RequestMapping的headers属性-->
<a th:href="@{/testHeaders}">测试headers属性</a><br>
```

启动服务器，测试结果：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710403104850-63f4c9fb-28ac-483a-b4c4-cdcea6b49e97.png#averageHue=%23fbfafa&clientId=u9e0c3730-20d1-4&from=paste&height=409&id=ua3cc6168&originHeight=409&originWidth=457&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21151&status=done&style=shadow&taskId=u3f738c6d-a1aa-4db7-a748-bc6b4bd9c3f&title=&width=457)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710403163821-dd5ae672-3b0a-4ae3-b978-48c8bef4f63a.png#averageHue=%23f0efee&clientId=u9e0c3730-20d1-4&from=paste&height=201&id=u82c3b349&originHeight=201&originWidth=687&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18669&status=done&style=shadow&taskId=u13270e6f-f713-458a-bbff-4d288fb22f1&title=&width=687)
将后端控制器中的headers属性值进行修改：
```java
@RequestMapping(value="/testHeaders", headers = {"Referer=http://localhost:8888/springmvc/"})
public String testHeaders(){
    return "testHeaders";
}
```
再次测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710403270750-77c19967-a2a8-423d-9fea-c9632e48cf8c.png#averageHue=%23e6c692&clientId=u9e0c3730-20d1-4&from=paste&height=279&id=u63c4ff7f&originHeight=279&originWidth=554&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20560&status=done&style=shadow&taskId=ue2bdd8ab-8480-4467-a9d4-dd814913f2e&title=&width=554)

其他情况自行测试！！！！

