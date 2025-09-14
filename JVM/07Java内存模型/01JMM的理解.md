![](assets/01JMM的理解/file-20250909095312061.png)

### 1.1原子性
![](assets/01JMM的理解/file-20250909103304331.png)
![](assets/01JMM的理解/file-20250909103254329.png)

系统级理解如下图所示

![](assets/01JMM的理解/file-20250909103951324.png)
![](assets/01JMM的理解/file-20250909104056605.png)

### 1.2可见性

![](assets/01JMM的理解/file-20250909104705324.png)![](assets/01JMM的理解/file-20250909104810807.png)
![](assets/01JMM的理解/file-20250909104908290.png)

**解决方法：volatile关键字**

![](assets/01JMM的理解/file-20250909105031721.png)
![](assets/01JMM的理解/file-20250909105250435.png)
![](assets/01JMM的理解/file-20250909105350357.png)

### 1.3有序性
![](assets/01JMM的理解/file-20250909105557213.png)
![](assets/01JMM的理解/file-20250909105739922.png)
![](assets/01JMM的理解/file-20250909105833528.png)

![](assets/01JMM的理解/file-20250909110100453.png)


**解决方案：试图用volatile来修饰，从而禁用指令重排**

![](assets/01JMM的理解/file-20250909110120441.png)


##### 有序性理解

![](assets/01JMM的理解/file-20250909110556001.png)


**多线程模式下的懒汉式单例模式的创建。这样写锁粒度较小，并发性较高**。即标准的双检加锁策略

![](assets/01JMM的理解/file-20250909111107757.png)
![](assets/01JMM的理解/file-20250909111137029.png)
* **上面的方法也是有问题，正确做法还需要给INSTANCE方法加volatile关键字**。具体原因如下


![](assets/01JMM的理解/file-20250909111639071.png)
![](assets/01JMM的理解/file-20250912182159505.png)





