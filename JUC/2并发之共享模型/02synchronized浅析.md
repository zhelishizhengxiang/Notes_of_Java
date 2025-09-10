**synchronized，即俗称的【对象锁】，它采用互斥的方式让同一时刻至多只有一个线程能持有【对象锁】，其它线程再想获取这个【对象锁】时就会阻塞住**。这样就能保证拥有锁的线程可以安全的执行临界区内的代码，不用担心线程上下文切换

### 1.synchronized的语法和基本使用

```java
synchronized(对象) // 线程1， 线程2(blocked)
{
	临界区
}
```
* **synchronized，即俗称的【对象锁】，它采用互斥的方式让同一时刻至多只有一个线程能持有【对象锁】，其它线程再想获取这个【对象锁】时就会阻塞住**
* **当该线程执行完后，会释放掉对象锁，然后唤醒一个正处于阻塞状态中的其他线程，然后由线程二获得该锁并执行临界区中的代码**
* **synchronized进行加锁都是配合一个多个线程共享的对象来完成的**

使用该关键字来访问共享资源的代码如下图所示  

![](assets/02synchronized浅析/file-20250725191301977.png)
* 可以看到结果从之前的不定值，变为现在的一直为理想的结果0。**两个线程必须争抢的是同一个锁，也就是同一个对象**
* 其理解如下图所示  
	![](assets/02synchronized浅析/file-20250725192314097.png)

* 理解：**synchronized 实际是用对象锁保证了临界区内代码的原子性，即临界区内的代码对外是不可分割的，不会被线程切换所打断。**
* **并且多个线程争夺统一资源时，这些线程对应的临界区必须都加锁。**



### 2.synchronized加在方法上
synchronized加在方法上是有两种语法，分别是加在成员方法上和静态方法上

加在普通方法上的语法如下
```java
class Test{
	public synchronized void test() {
	}
}
等价于
class Test{
	public void test() {
		synchronized(this) {
		
		}
	}
}
```
* **synchronized关键字加在成员方法上，相当于锁住的是this对象()。** 即两个线程同时调用**同一个对象**的 `synchronized` 方法时，会排队执行；但调用**不同对象**的同名同步方法时，互不影响

加在静态方法上的语法如下
```java
class Test{
	public synchronized static void test() {
	}
}
等价于
class Test{
	public void test() {
		synchronized(Test.class) {
		
		}
	}
}
```
* **synchronized关键字加在静态方法上，相当于锁住的是类对象()。** 即两个线程分别通过**不同对象**调用静态 `synchronized` 方法时，会排队执行（因为共享类锁）。
* **注意：锁住类对象不代表就锁住的this对象，两个对象是不同的对象。只有锁住的是相同的对象才会形成互斥**


### 3.线程八锁练习题
![](assets/02synchronized浅析/file-20250730153549243.png)
![](assets/02synchronized浅析/file-20250730153607987.png)
![](assets/02synchronized浅析/file-20250730154332627.png)
![](assets/02synchronized浅析/file-20250730154456158.png)![](assets/02synchronized浅析/file-20250730154730559.png)

![](assets/02synchronized浅析/file-20250730154908203.png)
![](assets/02synchronized浅析/file-20250730155001977.png)
![](assets/02synchronized浅析/file-20250730155051774.png)
 * 注意情况5和情况7中锁住的是不同对象，所以没有没有形成互斥