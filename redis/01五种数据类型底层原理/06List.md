**redis6list底层的数据结构为quicklist（是`linkedlist`和`ziplist`的结合）；而redis7的底层结构就是quicklist（是`linkedlist`和`listpack`的结合）**。quicklist粗粗的看就是一种双向链表

![](assets/06List/file-20251103203720655.png)

### 1.redis6底层结构quicklist

![](assets/06List/file-20251103204137261.png)
![](assets/06List/file-20251103204208078.png)
![](assets/06List/file-20251103204327424.png)
![](assets/06List/file-20251103204504576.png)
![](assets/06List/file-20251103205130484.png)

![](assets/06List/file-20251103205307066.png)
![](assets/06List/file-20251103205420210.png)
![](assets/06List/file-20251103205439726.png)

##### redis7的底层结构quicklist
![](assets/06List/file-20251103205529656.png)
![](assets/06List/file-20251103205729021.png)
