* **原生的Servlet API指的是：HttpServletRequest**
* 在**SpringMVC当中，一个Controller类中的方法参数上如果有HttpServletRequest，SpringMVC会自动将`**当前请求对象**`传递给这个参数，因此我们可以通过这个参数来获取请求提交的数据**。
* **同样的，如果方法中有HttpServletResponse response, HttpSession session，同样也会讲响应对象和session对象传过来**  
	![](assets/02原生的Servlet%20API获取请求参数（不建议使用）/file-20251225150658794.png)

在 register.html 中准备一个注册的表单：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<h3>用户注册</h3>
<hr>
<form th:action="@{/register}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    性别：
        男 <input type="radio" name="sex" value="1">
        女 <input type="radio" name="sex" value="0">
        <br>
    爱好：
        抽烟 <input type="checkbox" name="hobby" value="smoke">
        喝酒 <input type="checkbox" name="hobby" value="drink">
        烫头 <input type="checkbox" name="hobby" value="perm">
        <br>
    简介：<textarea rows="10" cols="60" name="intro"></textarea><br>
    <input type="submit" value="注册">
</form>
</body>
</html>
```

接下来在控制器添加一个方法来处理这个注册的请求：
```java
@PostMapping(value="/register")
public String register(HttpServletRequest request){
    // 通过当前请求对象获取提交的数据
    String username = request.getParameter("username");
    String password = request.getParameter("password");
    String sex = request.getParameter("sex");
    String[] hobbies = request.getParameterValues("hobby");
    String intro = request.getParameter("intro");
    System.out.println(username + "," + password + "," + sex + "," + Arrays.toString(hobbies) + "," + intro);
    return "success";
}
```

提供视图页面：
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>注册成功</title>
</head>
<body>
<h1>注册成功</h1>
</body>
</html>
```



测试：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710419827530-70740ef1-28a3-4766-9825-6d0cb5ebbf4a.png#averageHue=%23fafaf9&clientId=u9d1e8f4e-33cf-4&from=paste&height=361&id=ufbf91ed1&originHeight=361&originWidth=559&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11349&status=done&style=shadow&taskId=u8c5d516a-57fd-4c32-9b95-54bd42ad933&title=&width=559)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710419792573-b4d36148-dff4-45f6-ab97-e6de4e74a362.png#averageHue=%23f8f8f7&clientId=u9d1e8f4e-33cf-4&from=paste&height=186&id=u9190ef01&originHeight=186&originWidth=485&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9784&status=done&style=shadow&taskId=udb2f3c31-74ad-428b-adb6-434939d6e31&title=&width=485)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710419813547-5bb0218a-f11c-4438-982c-fc06964e7d45.png#averageHue=%23f7f4f0&clientId=u9d1e8f4e-33cf-4&from=paste&height=102&id=ud56d10fb&originHeight=102&originWidth=644&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19165&status=done&style=shadow&taskId=ufe093b32-8c7e-421c-9d97-c24aab5cb22&title=&width=644)

* **这样通过Servlet原生的API获取到提交的数据。但是这种方式不建议使用。**
* **因为方法的参数依赖Servlet原生API，Controller的测试将不能单独测试，必须依赖WEB服务器才能测试。
* **另外，换句话说，如果在SpringMVC中使用了原生的Servlet，你为什么还要用SpringMVC框架呢！！！！！**