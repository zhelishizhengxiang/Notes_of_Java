## 1.RequestParam注解的基本使用
该注解的源码如下图所示  
![](assets/03RequestParam注解获取/file-20250804212528538.png)
* **==RequestParam注解作用：将`请求参数`与方法上的`形参`映射==**。
* **该注解只能用在方法的参数中，并且name和value其实是一样的，只是取了个别名。**
* **value属性填入的是表中提交的参数的名字。也就是前端input框中name的属性值**
```java
@PostMapping(value = "/register")
public String register(
        @RequestParam(value="username")
        String a,
        @RequestParam(value="password")
        String b,
        @RequestParam(value="sex")
        String c,
        @RequestParam(value="hobby")
        String[] d,
        @RequestParam(name="intro")
        String e) {
    System.out.println(a);
    System.out.println(b);
    System.out.println(c);
    System.out.println(Arrays.toString(d));
    System.out.println(e);
    return "success";
}
```

**注意：对于@RequestParam注解来说，属性有value和name，这两个属性的作用相同，都是用来指定提交数据的name。  **  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710428008416-73b3a547-46ab-47bb-922c-b3d090e0cfc9.png#averageHue=%23fdfbf8&clientId=u9d1e8f4e-33cf-4&from=paste&height=331&id=uaf3c3ba7&originHeight=331&originWidth=494&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30298&status=done&style=shadow&taskId=u950651dd-684e-4de9-af50-2e721a82b8c&title=&width=494)

例如：发送请求时提交的数据是：name1=value1&name2=value2，则这个注解应该这样写：@RequestParam(value="name1")、@RequestParam(value="name2")


启动服务器测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710427890838-fe12392a-6fdf-4c94-8660-1bbee1f84a74.png#averageHue=%23fbfafa&clientId=u9d1e8f4e-33cf-4&from=paste&height=349&id=uc43a1942&originHeight=349&originWidth=544&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10924&status=done&style=shadow&taskId=u3fff6ffb-1196-4367-a348-00d141223a2&title=&width=544)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710427902078-fb8c772f-deae-49bc-93ec-0022ab4c8cd8.png#averageHue=%23f7f6f5&clientId=u9d1e8f4e-33cf-4&from=paste&height=163&id=u8a0409d9&originHeight=163&originWidth=455&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9552&status=done&style=shadow&taskId=u6e3d7af3-91cf-43bd-8905-38220eec21c&title=&width=455)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710427916293-aa47a26a-2d15-4b1b-ac6a-ada398655150.png#averageHue=%23fbf9f8&clientId=u9d1e8f4e-33cf-4&from=paste&height=209&id=u31439636&originHeight=209&originWidth=299&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12043&status=done&style=shadow&taskId=u24f9cf65-fb28-40b7-9084-cfef922a2d8&title=&width=299)

一定要注意： @RequestParam(value="name2") 中value一定不要写错，写错就会出现以下问题：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710428081389-9ac88bba-c37c-4fb8-9b9d-f7a9091b97ab.png#averageHue=%23fcfaf8&clientId=u9d1e8f4e-33cf-4&from=paste&height=604&id=ua16f46af&originHeight=604&originWidth=608&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76072&status=done&style=shadow&taskId=ub5f0221a-83b2-445d-8aa1-d0c787f1d54&title=&width=608)

测试结果：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710428139767-d3888c35-e2f8-407f-accb-f744a7098148.png#averageHue=%23e7c692&clientId=u9d1e8f4e-33cf-4&from=paste&height=272&id=u1fdff3b9&originHeight=272&originWidth=741&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24103&status=done&style=none&taskId=u5eb32a33-98fe-4768-b1e9-1fdb9c1c61d&title=&width=741)


## 2.RequestParam注解的required属性
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710465027479-caeb1d78-c92d-4fca-b9fa-80e6bcf06c7a.png#averageHue=%23fdfcf7&clientId=u9c5ec896-edd3-4&from=paste&height=181&id=u05ce0a11&originHeight=181&originWidth=670&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23639&status=done&style=shadow&taskId=uf614fe67-8ccc-4c8a-b773-840e46ac4f0&title=&width=670)
* **required属性用来设置该方法参数是否为必须的**。
* **默认情况下，这个参数为 `true`，表示方法参数是必需的。如果请求中缺少对应的参数，则会抛出异常。**
* **可以将其设置为`false`，false表示不是必须的。如果请求中缺少对应的参数，则方法的参数为null，并且不会由异常；如果请求参数中提供了对应，则该形参为真实提交的数据**。

测试，修改register方法，如下：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710468078605-3c6a2dd2-e9c4-4450-9712-02f11b5543d3.png#averageHue=%23fcfaf9&clientId=u9c5ec896-edd3-4&from=paste&height=660&id=ue64914c9&originHeight=660&originWidth=610&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83984&status=done&style=shadow&taskId=u2f988380-d51d-48ec-aec3-9d4be9fbe42&title=&width=610)


添加了一个 age 形参，**没有指定 required 属性时，默认是true，表示必需的，但前端表单中没有年龄age，我们来看报错信息**：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710468194109-56b8df42-2110-4b2b-9e73-064884f2e04b.png#averageHue=%23e8c792&clientId=u9c5ec896-edd3-4&from=paste&height=231&id=u1a3fb4dc&originHeight=231&originWidth=807&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18905&status=done&style=shadow&taskId=u4ccbff44-68a6-48f0-89b3-b7c2243d422&title=&width=807)

错误信息告诉我们：参数age是必需的。没有提供这个请求参数，HTTP状态码 400

如果将 required 属性设置为 false。则该参数则不是必须的，如果请求参数仍然未提供时，我们来看结果：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710468402437-7395c6e2-6ab4-4bdc-a66e-cb82811be4e4.png#averageHue=%23fcf9f7&clientId=u9c5ec896-edd3-4&from=paste&height=692&id=u3168e498&originHeight=692&originWidth=685&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92366&status=done&style=shadow&taskId=u8b6cd101-ae46-4d74-a6f8-d4d11007361&title=&width=685)


![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710468358266-82e27b39-b24a-4aca-902e-9a69c5630ca7.png#averageHue=%23f7f7f6&clientId=u9c5ec896-edd3-4&from=paste&height=158&id=u0b40f7d7&originHeight=158&originWidth=488&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9575&status=done&style=shadow&taskId=uf88cd954-2db8-4f8e-9c25-590db9f20f9&title=&width=488)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710468442095-a0aa03e0-390e-440c-b9db-61139b8098cb.png#averageHue=%23fcf9f8&clientId=u9c5ec896-edd3-4&from=paste&height=194&id=uf8a54914&originHeight=194&originWidth=290&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7036&status=done&style=shadow&taskId=uc3022386-a167-46b2-8eb2-b3614e9a495&title=&width=290)

通过测试得知，如果一个参数被设置为`不是必需的`，当没有提交对应的请求参数时，形参默认值null。

当然，如果请求参数中提供了age，则age为真实提交的数据：  

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710469986610-0f8a53d3-5e70-4127-a102-909bd6b75a46.png#averageHue=%23f8f6f3&clientId=u9c5ec896-edd3-4&from=paste&height=510&id=ue4678651&originHeight=510&originWidth=779&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83260&status=done&style=shadow&taskId=ufa33ce4b-0c36-46b9-9ea7-a4dffaa27c2&title=&width=779)



![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470042986-374bf055-aa7e-40d1-ae62-666d4b477ff9.png#averageHue=%23f9f8f8&clientId=u9c5ec896-edd3-4&from=paste&height=408&id=u447c8a81&originHeight=408&originWidth=586&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12406&status=done&style=shadow&taskId=u833a75a2-f685-4a14-a01e-2d0fcdd47ca&title=&width=586)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470056000-452b7435-23c0-4c3f-a1d0-e4b4b933a802.png#averageHue=%23f6f6f5&clientId=u9c5ec896-edd3-4&from=paste&height=159&id=uc12c6f79&originHeight=159&originWidth=438&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9528&status=done&style=shadow&taskId=ue4d2c2c4-25e6-420a-8501-7821e8f70d3&title=&width=438)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470068447-179bdbdc-7281-4db9-96ae-8c120f826898.png#averageHue=%23fcf9f8&clientId=u9c5ec896-edd3-4&from=paste&height=209&id=udaf54757&originHeight=209&originWidth=305&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9297&status=done&style=shadow&taskId=uc84010f3-4c31-40de-b8db-87c5f1b43df&title=&width=305)


## 3.RequestParam注解的defaultValue属性
* **defaultValue属性用来设置形参的默认值，当`没有提供对应的请求参数`或者`请求参数的值是空字符串""`的时候，方法的形参会采用默认值。如果有则采用请求参数的值**

![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470373422-d7c95422-71b8-4662-a99b-913343b8c59e.png#averageHue=%23fdfafa&clientId=u9c5ec896-edd3-4&from=paste&height=687&id=u2e8cc75d&originHeight=687&originWidth=1069&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99557&status=done&style=shadow&taskId=u4933e1a0-3f95-40cc-bf17-61982515665&title=&width=1069)

当前端页面没有提交email的时候：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470456573-54784174-401f-4fde-a7c3-9e3f88414d88.png#averageHue=%23f9f8f8&clientId=u9c5ec896-edd3-4&from=paste&height=359&id=ubffcf159&originHeight=359&originWidth=539&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10964&status=done&style=shadow&taskId=ud63d7f89-c02c-435e-820b-95db25ff1cb&title=&width=539)



![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470470975-d91804e7-a1ad-465f-a9dc-ed57e560d146.png#averageHue=%23f7f6f5&clientId=u9c5ec896-edd3-4&from=paste&height=160&id=u799975dc&originHeight=160&originWidth=464&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9631&status=done&style=shadow&taskId=uec3b5f22-29cb-4cd5-bd06-fceaa967784&title=&width=464)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470482292-3a68329a-02e6-4a41-b3ae-f3ac1cb9635d.png#averageHue=%23fcf7f6&clientId=u9c5ec896-edd3-4&from=paste&height=184&id=uc47cf34c&originHeight=184&originWidth=444&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9583&status=done&style=shadow&taskId=u6f72afa7-730c-4a05-8932-6a45b438432&title=&width=444)

当前端页面提交的email是空字符串的时候：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470529370-873dcc0f-40eb-4348-ac4b-82abf1d921f9.png#averageHue=%23f8f6f3&clientId=u9c5ec896-edd3-4&from=paste&height=495&id=u44487366&originHeight=495&originWidth=817&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83349&status=done&style=shadow&taskId=uf787e9d6-7a63-4e16-9ed1-d9af22106b6&title=&width=817)



![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470563357-648e4929-5b70-4e25-a364-3905c89da147.png#averageHue=%23f9f7f7&clientId=u9c5ec896-edd3-4&from=paste&height=390&id=ub92f2ca0&originHeight=390&originWidth=562&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11893&status=done&style=shadow&taskId=u421d8e1e-4c85-4232-ba03-c723fc67522&title=&width=562)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470573342-6a3b4497-74a9-44e0-a15b-add971551d64.png#averageHue=%23f7f7f6&clientId=u9c5ec896-edd3-4&from=paste&height=165&id=u5fb5dcde&originHeight=165&originWidth=477&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9628&status=done&style=shadow&taskId=u35f3d887-77a5-4135-bf1b-45090841453&title=&width=477)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470582878-a1d3fc60-f4fc-4f6c-95db-0cd924fa7f75.png#averageHue=%23fcf5f5&clientId=u9c5ec896-edd3-4&from=paste&height=202&id=u295200cd&originHeight=202&originWidth=339&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9879&status=done&style=shadow&taskId=ub5a7b2cb-2380-47f8-bf28-701d5ad9152&title=&width=339)



当前端提交的email不是空字符串的时候：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470628085-cb06b835-2098-4047-973f-5f572b6a09c0.png#averageHue=%23faf8f8&clientId=u9c5ec896-edd3-4&from=paste&height=390&id=u7cc385df&originHeight=390&originWidth=563&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13943&status=done&style=shadow&taskId=ue43fe266-9d34-4c26-9abd-11a160a8e04&title=&width=563)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470635669-d5c193c2-f62a-44a9-881f-066e96cc4059.png#averageHue=%23f7f6f5&clientId=u9c5ec896-edd3-4&from=paste&height=164&id=ufe1c8d5c&originHeight=164&originWidth=446&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9560&status=done&style=shadow&taskId=u8b9b58d6-aab9-47f5-bbc6-8fa24adf09b&title=&width=446)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710470647079-313b64ca-4a54-4581-8dc6-11f039e70477.png#averageHue=%23fcf6f5&clientId=u9c5ec896-edd3-4&from=paste&height=197&id=u9f7a0b6f&originHeight=197&originWidth=324&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9549&status=done&style=shadow&taskId=u79314bf9-c735-45b4-a893-133f9cc0a8a&title=&width=324)


