 
* **该注解的作用是：将`请求头信息`映射到`方法的形参上`**。
* 和RequestParam注解功能相似，RequestParam注解的作用：将`请求参数`映射到`方法的形参`上。
* **当然，对于RequestHeader注解来说，也有三个属性：value、required、defaultValue，和RequestParam一样，这里就不再赘述了**。

测试：
```java
@PostMapping("/register")
public String register(User user, 
                       @RequestHeader(value="Referer", required = false, defaultValue = "") 
                       String referer){
    System.out.println(user);
    System.out.println(referer);
    return "success";
}
```

执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710472685320-fa79ddc4-04e0-4f8e-b97e-56f3f28ee60f.png#averageHue=%23f9f3f1&clientId=u9c5ec896-edd3-4&from=paste&height=100&id=u968684d7&originHeight=100&originWidth=1131&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21660&status=done&style=shadow&taskId=uabce8eab-3132-4c53-bf57-fed7ab1cb6b&title=&width=1131)


