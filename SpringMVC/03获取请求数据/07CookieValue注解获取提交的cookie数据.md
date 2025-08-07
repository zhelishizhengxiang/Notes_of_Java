* **该注解的作用：将`请求提交的Cookie数据`映射到`方法形参`上**
* 同**样是有三个属性：value、required、defaultValue**

前端页面中编写发送cookie的代码：
```html
<script type="text/javascript">
    function sendCookie(){
        document.cookie = "id=123456789; expires=Thu, 18 Dec 2025 12:00:00 UTC; path=/";
        document.location = "/springmvc/register";
    }
</script>
<button onclick="sendCookie()">向服务器端发送Cookie</button>
```

后端UserController代码：
```java
    @GetMapping("/register")
    public String register(User user,
                           @RequestHeader(value="Referer", required = false, defaultValue = "")
                           String referer,
                           @CookieValue(value="id", required = false, defaultValue = "2222222222")
                           String id){
        System.out.println(user);
        System.out.println(referer);
        System.out.println(id);
        return "success";
    }
```

测试结果：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710473271244-7a95563a-fff4-458e-914f-25b314c78bd1.png#averageHue=%23fbf8f6&clientId=u9c5ec896-edd3-4&from=paste&height=128&id=ue186aba0&originHeight=128&originWidth=989&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21701&status=done&style=shadow&taskId=ub4dab47d-4e8a-43d9-a0af-7239f97f1e7&title=&width=989)


