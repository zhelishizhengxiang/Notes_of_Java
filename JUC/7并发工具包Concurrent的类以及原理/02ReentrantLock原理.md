### 1.综述
![](assets/02ReentrantLock原理/file-20250925143041261.png)
* ReentrantLock支持公平和非公平锁两种，所以有两种同步器，分别是非公平同步器和公平同步器

### 2.非公平锁实现原理

###### 加锁流程

![](assets/02ReentrantLock原理/file-20250925143249863.png)
* **先从构造器开始看，默认为非公平锁实现**。NonfairSync 继承自 AQS

当进行加锁时，该源码如图所示：    
![](assets/02ReentrantLock原理/file-20250925143440798.png)
![](assets/02ReentrantLock原理/file-20250926192441359.png)
![](assets/02ReentrantLock原理/file-20250926192634917.png)
![](assets/02ReentrantLock原理/file-20251119002916935.png)
![](assets/02ReentrantLock原理/file-20250926194128110.png)
![](assets/02ReentrantLock原理/file-20250926194036880.png)
* 加锁成功流程：**使用cas机制修改state状态。成功修改为1则设置owner线程该为当前线程。返回true**
* 加锁失败流程：**首先使用cas机制修改state状态失败，之后进入acquire(1)。会再次尝试加锁，但是会失败，之后会创建一个node节点对象，并将其加入到等待队列里去。之后完成自身的当前线程的阻塞**


图解如下：  
![](assets/02ReentrantLock原理/file-20250926192855895.png)
![](assets/02ReentrantLock原理/file-20250926193312649.png)
![](assets/02ReentrantLock原理/file-20250926193119786.png)
* 其中head和tail就是等待队列（是一个双向链表）的两个指针。第一个Node为哑元，起到占位的作用（首次创建等待队列中的node的时候，会创建2个，其最终dummy只起到占位的作用，此时队列长度位2，head指向dummy）
* `addWaiter` 函数的返回值是**新创建的 `Node` 实例**（封装了当前线程的节点）


![](assets/02ReentrantLock原理/file-20250926194328180.png)
* waitStatus状态为-1代表该节点有责任唤醒其后继节点

![](assets/02ReentrantLock原理/file-20250926195034200.png)![](assets/02ReentrantLock原理/file-20250926195133771.png)


###### 解锁竞争失败和成功流程

接着上面多个线程加锁竞争后的图解继续进行 。

相关源码如下图所示：  
![](assets/02ReentrantLock原理/file-20250926195855433.png)
![](assets/02ReentrantLock原理/file-20250926195943847.png)
![](assets/02ReentrantLock原理/file-20250926200053174.png)
![](assets/02ReentrantLock原理/file-20250926200613110.png)



图解如下图所示：  

![](assets/02ReentrantLock原理/file-20250926195611751.png)
![](assets/02ReentrantLock原理/file-20250927001825038.png)

注：其恢复之后的代码在下图所示的位置  

![](assets/02ReentrantLock原理/file-20250926194128110.png)
* 在该块代码一被唤醒，就会进入下一次循环。去执行tryAcquire()竞争锁。此时Thread1如果竞争成功了，设置thread1为owner线程，并且将state设置为1。
* 此时第一个if块为真，使用setHead()将该节点设置位头节点，将原来的dummy节点断开连接，将其中关联的线程设置为空（相当于一个新的dummy，最后的结果具体如上图所示）



如果竞争失败（非公平锁）：  
![](assets/02ReentrantLock原理/file-20250927002333493.png)
![](assets/02ReentrantLock原理/file-20250926201625999.png)
* 不公平指的时上图中的thread4并不在等待队列中。而是在锁开的一刹那，直接于thread1进行竞争。
* 如果该线程被唤醒后，就会进入下一次循环。去执行tryAcquire()竞争锁。此时Thread1如果竞争没有竞争成功，就会继续到parkAndCheckInterrupt()进行阻塞

###### 锁可重入原理
![](assets/02ReentrantLock原理/file-20250927102317796.png)
![](assets/02ReentrantLock原理/file-20250927102337460.png)![](assets/02ReentrantLock原理/file-20250927102356852.png)![](assets/02ReentrantLock原理/file-20250927102559808.png)
* 锁重入加锁时源代码如上图所示：**如果判断owner线程当前线程，就讲state++（如果一次锁重入，那么state就为2）**

![](assets/02ReentrantLock原理/file-20250927102823367.png)
![](assets/02ReentrantLock原理/file-20250927102845539.png)![](assets/02ReentrantLock原理/file-20250927103021081.png)
* 锁可重入原理的结果部分，讲state减去1判断是否为0，只有为0才会释放成功

###### 可打断原理

1. 不可打断模式：即lock()

![](assets/02ReentrantLock原理/file-20250927103252576.png)
![](assets/02ReentrantLock原理/file-20250927103313247.png)
![](assets/02ReentrantLock原理/file-20250927103856029.png)
![](assets/02ReentrantLock原理/file-20250927104030286.png)
![](assets/02ReentrantLock原理/file-20250927104253951.png)
* 进入park的线程，可以被其他线程调用interrupt()进行唤醒，并设置打断标记
* 在此模式下，即使它被打断，仍会驻留在 AQS 队列中，一直要等到获得锁后方能得知自己被打断（是继续运行，只是打断标记被设置为了true）


2. 可打断模式：调用lockInterruptably()

![](assets/02ReentrantLock原理/file-20250927104352416.png)
![](assets/02ReentrantLock原理/file-20250927104657056.png)

### 3.公平锁实现原理

首先放一下非公平锁的tryAcquire()内部逻辑    
![](assets/02ReentrantLock原理/file-20250927104858102.png)
![](assets/02ReentrantLock原理/file-20250927104955217.png)
* 当有线程尝试获得锁调用tryAcquire()时，直接使用cas去获得而不检查等待队列


那么公平锁对应的tryAcquire()实现如下：  
![](assets/02ReentrantLock原理/file-20250927105433970.png)
![](assets/02ReentrantLock原理/file-20250927105303106.png)
![](assets/02ReentrantLock原理/file-20250927105640914.png)
* **当使用后公平锁争抢时，会先去检查队列中是否有除dummy节点外的等待的节点并且不是当前线程，如果有则返回false，没有的话才会去竞争**
* 返回到外层后就会将其加入到等待队列中阻塞

### 4.条件变量实现原理

**每个条件变量其实就对应着一个等待队列，其实现类是 ConditionObject。他也是维护了一个双向链表，来存放那些不满足条件阻塞的线程**。（类似于cynchronized中waitset的作用）
###### await()原理
![](assets/02ReentrantLock原理/file-20250927105939343.png)
![](assets/02ReentrantLock原理/file-20250927111628163.png)
![](assets/02ReentrantLock原理/file-20250927110721259.png)
![](assets/02ReentrantLock原理/file-20250927111233876.png)
![](assets/02ReentrantLock原理/file-20250927111444255.png)

图解如下图所示：     
![](assets/02ReentrantLock原理/file-20250927110847726.png)
![](assets/02ReentrantLock原理/file-20250927111333556.png)
* 有可能该线程发生了锁重入，state>1.所以此处使用fullRelease(state)。底层直接release(state)，将节点对应线程上所有的锁都是释放掉

![](assets/02ReentrantLock原理/file-20250927111520862.png)
![](assets/02ReentrantLock原理/file-20250927131445362.png)
###### signal()原理
![](assets/02ReentrantLock原理/file-20250927131821511.png)
![](assets/02ReentrantLock原理/file-20250927132608005.png)
![](assets/02ReentrantLock原理/file-20250927133130963.png)
* 首先会判断当前线程是否是owner线程。因为只有持有锁的线程才可以唤醒或者阻塞线程。
* 有可能转移到竞争锁的等待队列时会转移失败，因为有可能在等待的线程会被打断或者是超时等待
* transferForSignal用于将该节点加入到等待队列当中。
* enq(node)方法就是把该节点加入到等待队列的尾部。如果成功的话返回的是该节点的前驱节点

图解如下图所示：  
![](assets/02ReentrantLock原理/file-20250927133333298.png)
![](assets/02ReentrantLock原理/file-20250927133457776.png)
![](assets/02ReentrantLock原理/file-20250927133556595.png)



