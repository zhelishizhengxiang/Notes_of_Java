### 1.定义和用法
![](assets/08ThreadLocal/file-20251116223137180.png)
![](assets/08ThreadLocal/file-20251116223343359.png)
* ThreadLocal变量成为线程局部变量。  

![](assets/08ThreadLocal/file-20251116225238851.png)
![](assets/08ThreadLocal/file-20251116224627107.png)
* initialValue()相当于用于给当前线程的线程局部变量赋初始值的。一般使用匿名内部类来创建并手动重写该方法设置每个线程局部变量的初始值。**但现实使用时不再用该方法，都是使用withInitial来替代**
* **如果不设置初始值，那么默认是返回null**
###### 使用案例

![](assets/08ThreadLocal/file-20251116225840799.png)
![](assets/08ThreadLocal/file-20251116225944382.png)


###### 使用要点
![](assets/08ThreadLocal/file-20251116230251152.png)
![](assets/08ThreadLocal/file-20251116230517104.png)
* **注意线程池场景下必须要使用remove()删除当前线程的线程局部变量，因为线程池涉及线程的复用，会导致内存泄漏或影响后续业务逻辑**    
	![](assets/08ThreadLocal/file-20251116230927641.png)


###### 总结
![](assets/08ThreadLocal/file-20251116231143929.png)


### 2.ThreadLocal底层源码解析

#### 2.1Thread、ThreadLocal、ThreadLocalMap三者的关系
![](assets/08ThreadLocal/file-20251116231602558.png)
![](assets/08ThreadLocal/file-20251116232104112.png)
* **当每new出来一个线程的时候，Thread对象中都会有一个ThreadLocal的实例对象作为其成员变量**。所以**每个线程在访问ThreadLocal变量实例的时候都会有自己的副本**

![](assets/08ThreadLocal/file-20251116232402430.png)
 ![](assets/08ThreadLocal/file-20251116232445341.png)
 * **ThreadLocalMap是ThreadLocal的静态内部类**。
 * **Map中的每个节点Entry是继承了弱引用WeakReference**()。并且key为ThreadLocal变量实例，Value是对应的值。   
![](assets/08ThreadLocal/file-20251116234335024.png)
![](assets/08ThreadLocal/file-20251116234401200.png)
#### 2.2源码分析

###### get()方法源码
![](assets/08ThreadLocal/file-20251116233432023.png)
![](assets/08ThreadLocal/file-20251116233443839.png)
![](assets/08ThreadLocal/file-20251116233451153.png)
![](assets/08ThreadLocal/file-20251116233641090.png)
![](assets/08ThreadLocal/file-20251116233703078.png)
![](assets/08ThreadLocal/file-20251116234008803.png)

###### set()底层源码
![](assets/08ThreadLocal/file-20251116234109132.png)
![](assets/08ThreadLocal/file-20251116233443839.png)
![](assets/08ThreadLocal/file-20251116233703078.png)
![](assets/08ThreadLocal/file-20251116234252537.png)


###### 底层结构总结
![](assets/08ThreadLocal/file-20251116235223767.png)

### 3.ThreadLocal的内存泄漏问题
 ![](assets/08ThreadLocal/file-20251117170701764.png)
 ![](assets/08ThreadLocal/file-20251117175037111.png)
###### 为什么Entry要继承弱引用
![](assets/08ThreadLocal/file-20251117175251089.png)![](assets/08ThreadLocal/file-20251117175935037.png)
![](assets/08ThreadLocal/file-20251117180454041.png)![](assets/08ThreadLocal/file-20251117180531813.png)
![](assets/08ThreadLocal/file-20251117180938695.png)
###### 源码分析
get()、set()和remove()会主动清理key为null的entry

![](assets/08ThreadLocal/file-20251117181024439.png)
![](assets/08ThreadLocal/file-20251117181145873.png)

![](assets/08ThreadLocal/file-20251117181301121.png)
![](assets/08ThreadLocal/file-20251117181312673.png)


![](assets/08ThreadLocal/file-20251117181405905.png)

### 4.总结
![](assets/08ThreadLocal/file-20251117181549204.png)![](assets/08ThreadLocal/file-20251117181746206.png)