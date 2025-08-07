* **HiddenHttpMethodFilter是Spring MVC框架提供的，专门用于RESTFul编程风格**。

实现原理可以通过源码查看其核心方法，具体如下图：    
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710981996209-5c66441b-0aa9-41a7-b71d-26b2ffb0f4f5.png#averageHue=%23fdfaf9&clientId=u1c5395e7-28c7-4&from=paste&height=587&id=u35d657b9&originHeight=587&originWidth=1291&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91768&status=done&style=shadow&taskId=u077ff919-ffea-49a1-9104-627c073a7e2&title=&width=1291)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710982160559-ffe20024-a10a-4aa2-b39e-44bebd0d3945.png#averageHue=%23e3d3b0&clientId=u1c5395e7-28c7-4&from=paste&height=134&id=u4b2501ea&originHeight=134&originWidth=699&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19516&status=done&style=shadow&taskId=ub9ce2e5e-f893-4b8c-8afc-1a59a08a49f&title=&width=699)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710982194265-720a0b49-aa95-475f-900b-7234280f5c9c.png#averageHue=%23fcfbfa&clientId=u1c5395e7-28c7-4&from=paste&height=93&id=u4e856ede&originHeight=93&originWidth=925&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14484&status=done&style=shadow&taskId=u88436ca4-4970-4d64-9b71-c8871fab154&title=&width=925)
* 通过源码可以看到，if语句中，首先判断是否为POST请求，如果是POST请求，调用`request.getParameter(this.methodParam)`。
* 可以看到`this.methodParam`是`_method`，这样就要求我们在提交请求方式的时候必须采用这个格式：`_method=put`。
* 获取到请求方式之后，调用了toUpperCase转换成大写了。因此前端页面中小写的put或者大写的PUT都是可以的。
* if语句中嵌套的if语句说的是，只有请求方式是 PUT,DELETE,PATCH的时候会创建HttpMethodRequestWrapper对象。而HttpMethodRequestWrapper对象的构造方法是这样的：  
	![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710984179119-96331e0b-ae39-45b0-bba1-b8db3ec7107f.png#averageHue=%23fefcfb&clientId=u1c5395e7-28c7-4&from=paste&height=395&id=udaf4f43c&originHeight=395&originWidth=905&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37994&status=done&style=shadow&taskId=u8c6216f0-6cfb-497c-802d-35c9446b8fe&title=&width=905)
* 这样method就从POST变成了：PUT/DELETE/PATCH。



**重点注意事项：CharacterEncodingFilter和HiddenHttpMethodFilter的顺序**

* 细心的同学应该注意到了，在HiddenHttpMethodFilter源码中有这样一行代码：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710984264334-7df83331-ddbb-4ead-a58c-cb4dc6c19ef6.png#averageHue=%23f9f6f3&clientId=u1c5395e7-28c7-4&from=paste&height=46&id=ue805f02f&originHeight=46&originWidth=614&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11849&status=done&style=shadow&taskId=u135178c5-b2d9-4d70-9fa7-91cb3e8dfc9&title=&width=614)
* 大家是否还记得，字符编码过滤器执行之前不能调用 request.getParameter方法，如果提前调用了，乱码问题就无法解决了。因为request.setCharacterEncoding()方法的执行必须在所有request.getParameter()方法之前执行。因此**这两个过滤器就有先后顺序的要求，在web.xml文件中，应该先配置CharacterEncodingFilter，然后再配置HiddenHttpMethodFilter。（Filter在xml中配置，谁先配置谁的优先级更高）**

