### 1.作用
作用：**信号量，用来限制能同时访问共享资源的线程上限。** 具体入下图所示  

![](assets/05Semaphere信号量/file-20251005132618186.png)
![](assets/05Semaphere信号量/file-20251005132728103.png)
* **获得信号量的线程会运行，当信号量被三个线程分配完后其他线程再来访问时会阻塞在acquire()。只有当有线程释放信号量后才可以被唤醒继续执行**


### 2.原理
###### acquire()
![](assets/05Semaphere信号量/file-20251005133434801.png)
![](assets/05Semaphere信号量/file-20251005133449461.png)
![](assets/05Semaphere信号量/file-20251005133505539.png)
![](assets/05Semaphere信号量/file-20251005133707294.png)
![](assets/05Semaphere信号量/file-20251005134448480.png)
![](assets/05Semaphere信号量/file-20251005134043562.png)
![](assets/05Semaphere信号量/file-20251005134113178.png)
![](assets/05Semaphere信号量/file-20251005134607292.png)
* **tryAcquireShared(arg)由子类来实现的结果如果是负数则为获取失败，如果是大于等于0代表获取成功，并且返回值代表的是剩余的资源数**

图解如下图所示：    
![](assets/05Semaphere信号量/file-20251005133625323.png)
![](assets/05Semaphere信号量/file-20251005134651555.png)


###### release()
![](assets/05Semaphere信号量/file-20251005134731195.png)
![](assets/05Semaphere信号量/file-20251005134746201.png)
![](assets/05Semaphere信号量/file-20251005134819884.png)
![](assets/05Semaphere信号量/file-20251005134915050.png)


图解如下图所示：  
![](assets/05Semaphere信号量/file-20251005135142486.png)