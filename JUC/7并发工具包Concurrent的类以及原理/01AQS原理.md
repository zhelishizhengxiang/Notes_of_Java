### 1.概述
![](assets/01AQS原理/file-20250925135956198.png)
![](assets/01AQS原理/file-20250925140103796.png)
![](assets/01AQS原理/file-20250925140229229.png)
* AQS翻译过来就是抽象的基于队列的同步器
* 将来主要通过继承AQS类来重用父类提供的这些功能

### 2.使用AQS来使用不可重入锁

基本的骨架如下：  
![](assets/01AQS原理/file-20250925140900583.png)
![](assets/01AQS原理/file-20250925140921633.png)

接着时首先对AQS同步器中三个方法的实现。    
![](assets/01AQS原理/file-20250925141705993.png)
![](assets/01AQS原理/file-20250925142023265.png)

![](assets/01AQS原理/file-20250925141820278.png)
![](assets/01AQS原理/file-20250925141931486.png)


测试    
![](assets/01AQS原理/file-20250925142530433.png)
![](assets/01AQS原理/file-20250925142659756.png)


![](../2共享模型之管程/assets/11可重入锁（ReentrantLock）/file-20250925142831906.png)