**底层是基于单向链表的结构**
### 1.基本的入队出队操作
![](assets/03LinkedBolockingQueue原理/file-20251007140907861.png)
![](assets/03LinkedBolockingQueue原理/file-20251007140953881.png)
![](assets/03LinkedBolockingQueue原理/file-20251007141100622.png)
* 对应于源码的enqueue()

出队时：对应于源码的deq()    
![](assets/03LinkedBolockingQueue原理/file-20251007141411522.png)![](assets/03LinkedBolockingQueue原理/file-20251007141424989.png)
![](assets/03LinkedBolockingQueue原理/file-20251007141439565.png)


### 2.加锁分析
![](assets/03LinkedBolockingQueue原理/file-20251007141928680.png)
![](assets/03LinkedBolockingQueue原理/file-20251007142018682.png)
![](assets/03LinkedBolockingQueue原理/file-20251007142308568.png)
### 3.put和take分析
![](assets/03LinkedBolockingQueue原理/file-20251007143008413.png)

take的流程与put完全相反：    
![](assets/03LinkedBolockingQueue原理/file-20251007143106182.png)


### 4.与ArrayBlockingQueue性能比较
![](assets/03LinkedBolockingQueue原理/file-20251007143239652.png)
* 综上所述，LinkedBlockingQueue的性能更好