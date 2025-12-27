注：该类的api与HashMap的API是基本一样的只是内部实现不同。

* **但是ConcurrentHashMap虽然是线程安全类，能够保证每一个方法都是原子操作。但是这些线程安全方法的组合并不是线程安全的。**
* **ConcurrentHashMap还有一个computeIfAbsent( `K key, Function<? super K, ? extends V> mappingFunction`)，代表key存在时则直接返回value；如果缺少一个值，则计算生成一个value，然后将key-val放入map，最后返回计算出的 value。
* Function用于写有一个参数一个返回结果的函数式接口  
* **CocnurrentHashMap的遍历、读取（get）和size()都是弱一致性，如法保证100%正确**

大总结：  
![](assets/02ConcurrentHashMap原理/file-20251006190555218.png)
### 1.JDK7 HashMap并发死链

注：j**dk7中hashmap是由数组+链表**的数据结构组成。使用拉链法解决哈希冲突时，**是将新节点加入链表头部；JDK8是放在链表尾部（可以避免死链问题，但是会有其他的问题（比如扩容丢数据）。

**并发死链：在多线程下JDK7 HashMap进行扩容时，会造成并发死链的问题。使得程序直接卡死，直接内存OutofMemory**


###### 测试代码
![](assets/02ConcurrentHashMap原理/file-20251006164225065.png)
![](assets/02ConcurrentHashMap原理/file-20251006164310093.png)


###### 死链复现

在HashMap第590行源码加断点，具体如图所示。该方法用于扩容时调用完成从旧table到新table元素的迁移

![](assets/02ConcurrentHashMap原理/file-20251006164338260.png)
![](assets/02ConcurrentHashMap原理/file-20251006164639540.png)

此时下标为1的数组的链表入下图所示，新加进来的节点放在链表头    
![](assets/02ConcurrentHashMap原理/file-20251006164942544.png)


![](assets/02ConcurrentHashMap原理/file-20251006165256364.png)
![](assets/02ConcurrentHashMap原理/file-20251006165311180.png)
* 这是为了让thread1去完成扩容    
![](assets/02ConcurrentHashMap原理/file-20251006165458381.png)

![](assets/02ConcurrentHashMap原理/file-20251006165931493.png)
![](assets/02ConcurrentHashMap原理/file-20251006165833910.png)
* 为什么thread1执行完链表变成这样了呢，因为 Thread-1 扩容时链表也是后加入的元素放入链表头，因此链表就倒过来了


此时让thread0来执行，他还是停在594行，并且e和next引用的对象仍然时1和35  
![](assets/02ConcurrentHashMap原理/file-20251006165458381.png)
![](assets/02ConcurrentHashMap原理/file-20251006171306567.png)
![](assets/02ConcurrentHashMap原理/file-20251006171241307.png)
![](assets/02ConcurrentHashMap原理/file-20251006171406514.png)
* 相当于1和35变成了一个环，程序就会在这里一直循环。形成死链

![](assets/02ConcurrentHashMap原理/file-20251006171518458.png)


### 2.JDK8ConcurrentHashMap原理

**JDK8ConcurrentHashMap底层依然是链表+数组+红黑树，链表转红黑树的阈值依然是8（前提是扩容到64），红黑树转链表节点依然为6**
#### 2.1重要属性、内部类和内部方法
![](assets/02ConcurrentHashMap原理/file-20251006171827175.png)
![](assets/02ConcurrentHashMap原理/file-20251006174431535.png)
* 这里的初始化指的是桶数组的初始化
* 初始化或者扩容完成后，就变为下一次扩容阈值大小，即容量的四分之三（默认情况下）
* **默认情况下如果是无参构造sizeCtl才会为默认0，其他构造器在初始化前都是待创建数组的容量**


![](assets/02ConcurrentHashMap原理/file-20251006172133716.png)


![](assets/02ConcurrentHashMap原理/file-20251006172148864.png)![](assets/02ConcurrentHashMap原理/file-20251006172226535.png)
* 还有一个用户是在扩容时调用get()时，程序如果遇到ForwardingNode，程序就知道应该去新的table获取数据

![](assets/02ConcurrentHashMap原理/file-20251006172818380.png)
* TreeBin是红黑树的头节点，TreeNode是红黑树的其他节点

![](assets/02ConcurrentHashMap原理/file-20251006173035299.png)


#### 2.2构造器分析


![](assets/02ConcurrentHashMap原理/file-20251006173152484.png)
* ConcurrentHashMap的构造器主要由上图所示。挑一个三个参数的来讲解。

![](assets/02ConcurrentHashMap原理/file-20251006173937239.png)
*  int initialCapacity, float loadFactor, int concurrencyLevel分别为初始容量、负载因子（扩容因子）和并发度。
* **JDK8中的ConcurrentHashMap使用了懒惰初始化，并不是在创建对象时旧创建出桶数组，会在将来第一次用到的时候才会去第一次创建**
* sizeCtl是将2的n次幂复制给sizeCtl

#### 2.3get流程


![](assets/02ConcurrentHashMap原理/file-20251006181237994.png)![](assets/02ConcurrentHashMap原理/file-20251006181315366.png)
![](assets/02ConcurrentHashMap原理/file-20251006181344992.png)
* spread 方法能确保key的hashcode返回结果是正数。因为负数用于做一些其他的用处
* ForwardingNode和TreeBin的hash()下来的值就是负数。其中扩容中的ForwardingNode恒为-1，树TreeBin恒为-2
* 调用 find 方法会根据是正在扩容中还是该节点是一棵树来分情况查找节点。


#### 2.4put流程
以下数组简称（table），链表简称（bin）

###### 总体流程

![](assets/02ConcurrentHashMap原理/file-20251006181510037.png)
![](assets/02ConcurrentHashMap原理/file-20251006182428796.png)
![](assets/02ConcurrentHashMap原理/file-20251006183751596.png)
![](assets/02ConcurrentHashMap原理/file-20251006183852970.png)
* onlyIfAbsent为真表示只有第一次put该键值的时候才会把该kv放入map，如果以后put相同的key，则不会做任何的操作，即不会新值覆盖旧值。默认为false。
* 从上图中可以看出，**ConcurrentHashMap不允许有空的键和值，而HashMap是允许的**
* **helpTransfer()会去锁住当前的旧链表，然后去帮忙扩容**
* else块当中代表肯定是发生了桶数组下标的冲突，此时由两种情况，要么是链表要么所示红黑树
* 增加size计数是通过类似于LongAdder累加器的机制来统计sieze计数并提高效率（后面会细讲）。并且还会进行扩容

###### initTable()

![](assets/02ConcurrentHashMap原理/file-20251006184524946.png)
* **initTable()初始化内部 table 使用了 cas, 无需 synchronized**

###### addCount()
![](assets/02ConcurrentHashMap原理/file-20251006185420488.png)
![](assets/02ConcurrentHashMap原理/file-20251006185508601.png)
* **addCount()用于增加Map中的元素个数的计数**
* rs << RESIZE_STAMP_SHIFT) + 2)如果复制成功肯定是一个负数


#### 2.5size流程
![](assets/02ConcurrentHashMap原理/file-20251006185903320.png)
* **ConcurrentHashMap中size()的计数是有一定误差，是弱一致的。（高并发场景）**


###### 2.6Transfer流程
![](assets/02ConcurrentHashMap原理/file-20251006190352754.png)
* nextTable如果为空会去创建。
* 如果该链表处理完，就会替换成ForwardingNode。
* 如果发现该头节点以及是ForwardingNode，就会去下一轮循环处理下一个节点。
* 如果节点是普通节点，那么旧做普通节点的移动工作，如果是树，那么旧走树节点的搬迁逻辑。


### 3.JDK7ConcurrentHashMap

![](assets/02ConcurrentHashMap原理/file-20251007113224719.png)

**每个segment集成自ReentrantLock，可以直接将其理解为一把锁**


#### 3.1构造器分析

以默认segments数组大小为16进行讲解。

![](assets/02ConcurrentHashMap原理/file-20251007113629226.png)
* **上图中的segmentshift和segmentMask主要是用于移位和掩码运算确定k到底对应哪个segment中的哪个数组**。  
	![](assets/02ConcurrentHashMap原理/file-20251007113930125.png)
* Segment构造器初始化并不是懒惰初始化。初始化完的场景如下图所示  
	![](assets/02ConcurrentHashMap原理/file-20251007113717836.png)


#### 3.2put流程

![](assets/02ConcurrentHashMap原理/file-20251007114213800.png)
![](assets/02ConcurrentHashMap原理/file-20251007114927985.png)
![](assets/02ConcurrentHashMap原理/file-20251007114956767.png)
* 首先进入的是ConcurrentHashMap的put流程，之后进入的是segment的put流程。
* **注意：ConcurrentHashMap都是把新节点放在链表头的**
* rehash()就是用于扩容的函数。

#### 3.3rehash()流程
![](assets/02ConcurrentHashMap原理/file-20251007115909272.png)
![](assets/02ConcurrentHashMap原理/file-20251007115943932.png)
* 在put中hi行rehash时，会将准备put添加的节点传进来，在扩容后再进去添加。

#### 3.4get流程

![](assets/02ConcurrentHashMap原理/file-20251007120355189.png)
* (Segment<K,V>)UNSAFE.getObjectVolatile(segments, u))保证可见性，其实就是jdk8中tabAt()方法的内部实现。所以二者在该处做的处理都一样的，只不过jdk8对其做了封装。

#### 3.5size()计算流程
![](assets/02ConcurrentHashMap原理/file-20251007121011010.png)